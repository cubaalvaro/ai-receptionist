# 6) Retell AI integration plan & n8n workflows

Contains webhooks, payloads, pseudocode, and localization details.

Sessions & tools

Create session: choose voice (ES-ES, ES-MX variants), temperature, latency preset, interruption settings (barge-in on, stop-on-voice).
Tools schema: declarative “functions” with JSON schema; Retell triggers tool_call → n8n executes and replies with tool_result.
n8n role: tool executor/state machine; keeps conversation state and business invariants.
Webhooks & signed callbacks

Use Retell webhooks for session_started, tool_call, transfer_to_human, session_ended. Include HMAC signature (shared secret) and Idempotency-Key header and correlation IDs. Retries: exponential backoff; accept-once semantics.
Sample payloads (abridged JSON)

session_started
{
  "event": "session_started",
  "session_id": "rt-123",
  "caller": { "ani": "+3491XXXXXXX", "locale": "es-ES" },
  "consent": { "jurisdiction": "ES", "recording_enabled": true }
}
tool_call
{
  "event": "tool_call",
  "session_id": "rt-123",
  "tool": "create_calendar_event",
  "args": { "name": "María Gómez", "when": "2025-10-22T10:00:00+02:00", "channel":"phone" }
}
n8n returns tool_result
{
  "session_id": "rt-123",
  "tool": "create_calendar_event",
  "result": { "status": "ok", "calendar_id":"A-1", "event_id":"evt_987", "join":"tel:+3491...." }
}
transfer_to_human
{
  "event": "transfer_to_human",
  "session_id": "rt-123",
  "target": { "sip":"sip:frontdesk@pbx.example.com" },
  "whisper": { "intent":"booking", "summary":"Cita mañana 10:00, María Gómez", "crm_id":"lead_2025-10-21_001" }
}
session_ended
{ "event":"session_ended","session_id":"rt-123","reason":"caller_hangup","dur_s":362 }
n8n artifacts

Nodes (core): Webhook (Retell in) → Function (HMAC verify + route) → Switch (event type) → Tool workers (HTTP Request, Google/Microsoft Calendar, CRM API, Payments/Tickets, Twilio SMS/WhatsApp) → Return (tool_result) → Postgres/BigQuery log → Metrics (HTTP).
Workflow (a) Intent→Schedule→CRM note (pseudo-JSON):
{
 "trigger":"webhook:/retell",
 "verify":"HMAC",
 "routes":{
   "tool_call:create_calendar_event":{
     "calendar.create": { "provider":"google", "account":"svc-es" },
     "crm.upsertLead": {},
     "respond.tool_result": {}
   }
 }
}
Workflow (b) Lead capture → qualification → warm transfer + whisper: same pattern; resolve human target via schedule/queue, send whisper context, then transfer_to_human.
Workflow (c) Voicemail fallback: on busy/no agents, branch to CPaaS voicemail; transcript via Retell/CPaaS transcription; follow-up via Twilio SMS/WhatsApp or email.
Localization

Prompt packs per variant (ES-ES, ES-MX, ES-CO, ES-AR, ES-CL, ES-PE), with slot-filling for names, addresses, and national IDs (NIF/NIE; CURP; RUT/RUN; CUIL/CUIT; DNI). Pronunciation and disambiguation dictionaries maintained in repo.
