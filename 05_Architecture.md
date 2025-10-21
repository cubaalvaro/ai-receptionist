# 5) Technical architecture (n8n + Retell AI)

Includes call flow diagrams, data flow, HA layout, and sequence diagrams.

(a) End-to-end call flow (ASCII)
PSTN ──> CPaaS (SIP/Twilio/Infobip)
          |  (SIP INVITE / Webhook)
          v
     Retell AI Realtime Agent  <──► WebRTC/PCM 16kHz, barge-in
          |  (webhooks: session.start/tool.call/transfer/end)
          v
       n8n (webhook/queue mode) ──► Business APIs/DB (CRM, EHR-lite, tickets, payments)
          |                                     |
          |<──────── context/results ───────────|
          v
   Human Handoff (SIP softphone/call center)  ◄── Whisper/context
          |
          v
   Observability & Data Lake (logs, metrics, redacted transcripts/recordings)
(b) Data flow & storage classification (ASCII)
[Audio RTP/WebRTC] (transient) -> Retell streaming
[Partial transcripts] (ephemeral buffer) -> redaction pipeline -> stored per retention
[Tool payloads] (PII-minimized) -> n8n queue -> vendor APIs
[Metadata] (session IDs, timings) -> metrics TSDB
[Recordings] (optional, dual-channel) -> CPaaS storage (encrypted) w/ retention policy
(c) High-availability layout (ASCII)
Multi-Region Ingress: EU-West (Spain) + US-East + LATAM (via CPaaS PoPs)
         |
   Stateless Retell sessions (auto-scale) ── Circuit Breakers ── DLQ (tool calls)
         |
     n8n cluster (queue mode + Redis/RabbitMQ + Postgres HA)
         |
   Observability stack (OpenTelemetry -> log store + metrics + traces)
Responsibilities & SLAs

Retell AI: Realtime ASR/LLM/TTS; barge-in; session control; webhooks; PII redaction; configurable storage. SLO: sub-second first token; sub-300ms turn latency; GDPR/HIPAA posture.
n8n: Tool execution, state, retries/idempotency, orchestration; queue mode horizontal scaling; webhook trigger.
CPaaS (Twilio/Infobip/SIP): Numbers, PSTN bridging, call recording, warm transfer.
Sequence (barge-in + tool call with budgets)
Caller -> CPaaS: INVITE/answer ................. 600–1200 ms
CPaaS -> Retell: media starts .................. 50–150 ms
Retell ASR first partial ....................... 150–300 ms
LLM first token (streaming) .................... 150–400 ms
TTS start speaking ............................. 120–250 ms
[barge-in] speech detect -> stop TTS ........... <100 ms
tool.call webhook -> n8n ....................... 20–50 ms
n8n tool exec -> external API .................. 150–600 ms (calendar/CRM)
tool.result -> Retell -> TTS reply ............. 200–400 ms
Transfer to human (SIP refer/bridge) ........... 500–2000 ms
