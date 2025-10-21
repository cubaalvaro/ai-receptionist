# 16) Appendices

Glossary, formulas, n8n/Retell configs, legal checklists, launch lists.

A) Glossary (telephony/voice AI)
Barge-in, dual-channel recording, DLQ, SLO/SLA/SLA breach, DSR, DPIA, CQRS, etc.

B) Assumptions & formulas
€/min = sum(Unit BOM rows). Monthly = minutes × €/min + fixed. FX at ECB rate on 2025-10-20.
C) Example n8n workflow & Retell config (snippets)
Retell session config (ES-ES / fast latency)

{
  "voice": "es-ES-female-01",
  "locale": "es-ES",
  "temperature": 0.3,
  "latency_preset": "low",
  "interruptions": { "barge_in": true, "stop_on_speech": true },
  "tools": [
    {"name":"create_calendar_event","schema":{}},
    {"name":"lookup_ticket","schema":{}},
    {"name":"handoff","schema":{"agent_queue":"string"}}
  ],
  "webhook_url": "https://n8n.example.com/webhook/retell",
  "auth_signature": "hmac-sha256"
}
n8n webhook verifier (pseudo):

if (!verifyHmac($json.headers, $binary.raw)) return $respond(401);
routeBy($json.event);
D) Country legal checklists (concise)
Spain (GDPR/LOPDGDD): Inform of recording & purpose; allow no-recording path if requested; DSRs: access, deletion, portability via n8n; DPA with vendors; SCCs if exporting data outside EEA.
Mexico: LFPDPPP/INAI — privacy notice, consent for processing/transfer; rights ARCO; keep call purposes explicit.
Colombia: Law 1581/2012 — authorization & database registration if applicable; SIC guidance.
Argentina: Law 25.326 — prior consent; international transfer rules (AAIP).
Chile: Current law + reform bill — adopt conservative consent practice.
Peru: Law 29733 — consent; ANPDP guidance.
E) Launch checklists
Spain (concise):

Numbers & routing; 2) Consent scripts (ES-ES); 3) Retell session presets; 4) n8n workflows (FAQ, scheduling, CRM); 5) DSR runbooks; 6) Observability dashboards; 7) Pilot UAT & go-live; 8) Canary & rollback plan.
LATAM reusable:

Country taxes & billing; 2) Local consent wording; 3) Numbers & WhatsApp templates; 4) Prompt localization; 5) Legal DPIA & DPA/SCCs; 6) Pilot cohort & QA; 7) Canaries; 8) Support hours local TZ.
