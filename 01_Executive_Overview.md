# 1) Executive overview
*(summary of strategy, MVP scope, performance, and rationale)*

Refer to the roadmap master file for details.

Strategy: Launch a GDPR-first realtime voice AI receptionist for Spain, then expand to Mexico → Colombia → Chile → Peru → Argentina. Use Retell AI for the voice agent (ASR/LLM/TTS + barge-in, call control) and n8n as the tool-executor/state machine for APIs, scheduling, CRM, and compliance automations.
Why Retell + n8n: Retell provides production-grade, low-latency agents with audit trails, PII redaction, and GDPR posture; n8n gives open, self-hostable orchestration with webhook/queue mode and easy integration to any API.
MVP scope (ES/ES-MX): Call routing, FAQs, calendar scheduling, lead capture/qualification, ticket/order lookups, voicemail fallback, warm transfer to humans; DTMF and dual-channel recording as accessibility/robustness features.
Performance targets: p95 call connect ≤3s; first response ≤1.2s; barge-in turn latency <300ms; ASR WER ≤10%; availability 99.5% (MVP) ↑ 99.9% by month 6.
Compliance: GDPR/LOPDGDD baseline with DPA/SCCs; configurable transcript/recording retention & redaction; EU data-subject rights (DSRs) SLAs and audit logs across Retell/n8n/CPaaS.
Go-to-market: Spain pilots in weeks 5–8 with SMB services/clinics/beauty/home services; expand by playbook country-by-country (telephony + legal checklist).
12-month OKR program: North-Star = AI receptionist that resolves ≥60% of inbound without human while maintaining customer-perceived quality (CSAT proxy ≥4.2/5). OKR cadence: quarterly planning; weekly check-ins with confidence scores and mid-quarter re-basing.
TCO ballpark (EUR, p=80/20 in/outbound, Twilio voice, Retell Starter/Business, n8n Cloud Pro; 1 EUR≈1.1655 USD as of Oct 20, 2025):
1k mins/mo: ~€205–€410 (≈$239–$478)
10k mins/mo: ~€1,850–€3,600 (≈$2,157–$4,194)
100k mins/mo: ~€16,500–€32,000 (≈$19,240–$37,296)
(range explains model/telco variance; full BOM in §10).
Key risks: Latency under load, accents/noise WER, consent/recording notices per country, calendar/CRM API brittleness, vendor lock-in; mitigations in §§9 & 15.
Single best reason for this stack: Fastest path to validated learning and operational excellence with minimal lock-in: Retell handles the realtime/voice hard parts; n8n keeps business logic portable (Lean Startup: optimize time through Build–Measure–Learn).
Expected KPI lift by month 6: 55–70% containment, 20–35% faster answer-to-resolution, missed-call rate ↓ 40–60%, human transfer success ≥95% with whisper/context pass; p95 latency <250ms intra-turn.
