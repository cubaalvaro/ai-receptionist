# 9) Performance engineering & SRE

Latency budgets, autoscaling, observability, reliability patterns.

Latency budget (targets)

Call setup (PSTN→agent): 0.6–1.2s
First token (ASR→LLM→TTS): ≤1.2s
Barge-in stop + turn latency: <300ms
Tool round-trip (n8n + API): typical 150–600ms; budget spikes to 1.2s with retries.
Autoscaling & backpressure

Retell sessions: horizontal autoscale by active streams & CPU; shed load to “please hold” prompt if saturation.
n8n queue workers: autoscale by queue depth/age; circuit-break slow tools; DLQ poison messages.
Degrade: switch to DTMF or voicemail when external APIs exceed SLOs.
Reliability patterns

Retries with exponential jittered backoff; idempotency keys (session_id + event_id).
Circuit breakers around CRM/calendar; bulkheads per integration.
DLQs with replay; rate-limiters per vendor.
Observability

Structured JSON logs incl. correlation IDs (cp-call-id, retell-session-id, n8n-exec-id).
OpenTelemetry traces across CPaaS → Retell → n8n → API; p50/p95 dashboards; redaction at log sink.
Session replays using redacted transcripts + dual-channel audio.
