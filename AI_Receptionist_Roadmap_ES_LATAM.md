# AI Receptionist Assistant (Spain + LATAM) — Technical & Strategic Roadmap  
*(n8n orchestration + Retell AI realtime voice agent + SIP/Twilio/Infobip)*  
VP Product & Lead Solutions Architect (ES/EN)

---

## 1) Executive overview (≤12 bullets)

- **Strategy:** Launch a *GDPR-first* realtime voice AI receptionist for Spain, then expand to Mexico → Colombia → Chile → Peru → Argentina. Use **Retell AI** for the voice agent (ASR/LLM/TTS + barge-in, call control) and **n8n** as the **tool-executor/state machine** for APIs, scheduling, CRM, and compliance automations.  
- **Why Retell + n8n:** Retell provides production-grade, low-latency agents with audit trails, PII redaction, and GDPR posture; n8n gives open, self-hostable orchestration with webhook/queue mode and easy integration to any API.  
- **MVP scope (ES/ES-MX):** Call routing, FAQs, calendar scheduling, lead capture/qualification, ticket/order lookups, voicemail fallback, warm transfer to humans; DTMF and dual-channel recording as accessibility/robustness features.  
- **Performance targets:** p95 call connect ≤3s; first response ≤1.2s; barge-in turn latency <300ms; ASR WER ≤10%; availability 99.5% (MVP) ↑ 99.9% by month 6.  
- **Compliance:** GDPR/LOPDGDD baseline with DPA/SCCs; configurable transcript/recording retention & redaction; EU data-subject rights (DSRs) SLAs and audit logs across Retell/n8n/CPaaS.  
- **Go-to-market:** Spain pilots in weeks 5–8 with SMB services/clinics/beauty/home services; expand by playbook country-by-country (telephony + legal checklist).  
- **12-month OKR program:** North-Star = *AI receptionist that resolves ≥60% of inbound without human* while maintaining customer-perceived quality (CSAT proxy ≥4.2/5). OKR cadence: quarterly planning; weekly check-ins with confidence scores and mid-quarter re-basing.  
- **TCO ballpark (EUR, p=80/20 in/outbound, Twilio voice, Retell Starter/Business, n8n Cloud Pro; 1 EUR≈1.1655 USD as of Oct 20, 2025):**  
  - **1k mins/mo:** ~€205–€410 (≈$239–$478)  
  - **10k mins/mo:** ~€1,850–€3,600 (≈$2,157–$4,194)  
  - **100k mins/mo:** ~€16,500–€32,000 (≈$19,240–$37,296)  
  (range explains model/telco variance; full BOM in §10).  
- **Key risks:** Latency under load, accents/noise WER, consent/recording notices per country, calendar/CRM API brittleness, vendor lock-in; mitigations in §§9 & 15.  
- **Single best reason for this stack:** Fastest path to **validated learning** and **operational excellence** with minimal lock-in: Retell handles the realtime/voice hard parts; n8n keeps business logic portable (Lean Startup: optimize time through **Build–Measure–Learn**).  
- **Expected KPI lift by month 6:** 55–70% containment, 20–35% faster answer-to-resolution, missed-call rate ↓ 40–60%, human transfer success ≥95% with whisper/context pass; p95 latency <250ms intra-turn.

---

## 2) Market & ICP definition (Spain & LATAM)

**ICPs & nuances**

| Segment | Spain (ES-ES) | Mexico (ES-MX) | Colombia (ES-CO) | Argentina (ES-AR) | Chile (ES-CL) | Peru (ES-PE) |
|---|---|---|---|---|---|---|
| SMB services (clinics, beauty, repairs, hospitality) | Formal greetings (“Buenos días, ¿en qué puedo ayudarle?”), consent notice upfront | “¿Bueno?” opening common; cell-first culture | Warm style; “¿con quién tengo el gusto?” | Tutear vs usted per brand; lunfardo variants | Direct, concise; RUT patterns | DNI formats; consent phrasing |
| Real estate/auto dealers | Peak 12–14h & 17–20h (CET) | Peak 11–13h & 16–19h (local) | Peak 9–12h & 14–17h | Appointment clustering; WhatsApp heavy | Clear price/IVA talk | Strong WhatsApp |
| Multi-location retail | Nationwide IVR; store locator | Multi-brand lines; WhatsApp templates | Regional accents (paisas, costeño) | Inflation notes; promotions cadence | Clear returns policy | Delivery windows |

**Competitive scan**: Retell’s native agent + webhooks + PII redaction and SOC2/ISO claims; our **differentiator** is **n8n-based tool execution** and a **regulatory operating model**.

**Pricing/packaging hypotheses**  
- **Base:** €0.XX/min usage + €15–€25/line/month + setup.  
- **Tiers:** *Starter* (≤1k mins), *Growth* (≤10k), *Scale* (≤100k+), with **tiered support SLAs** and **CS/Ops hours in local timezones**.  
- **Localization:** Bill in EUR for EU, local currency for LATAM where feasible; taxes: **IVA (ES 21%), IVA MX (16%), IGV Peru (18%), IVA AR, IVA CL**; pass through CPaaS taxes as separate line items.  
- **Add-ons:** Call recording/transcription, advanced analytics, custom redaction packs, WhatsApp follow-ups, premium voices.

---

## 3) Lean Startup strategy & validation plan

**Horizons & falsifiable hypotheses**

| Horizon | Weeks | Example hypotheses | Success criteria (falsifiable) |
|---|---:|---|---|
| **Pre-MVP** | 0–4 | H1: ≥50% of inbound intents (routing/FAQ/scheduling) can be handled with ≤1.2s first response; H2: Users accept consent prompts without drop-off >5% | p95 FR ≤1.2s; barge-in latency <300ms; consent drop-off ≤5%; WER ≤10% |
| **Beta** | 5–12 | H3: Containment ≥50% at 4–6 min avg handle; H4: Warm transfers succeed ≥95% with <5s time-to-agent | Containment ≥50%; transfer success ≥95%; CSAT proxy ≥4.2/5 |
| **Scale** | 4–12 mo | H5: Gross margin ≥65% at 10k–100k mins; H6: 99.9% avail with multi-region ingress | GM ≥65%; SLO: 99.9%; p95 intra-turn <250ms |

**Innovation accounting & cohorting:** Standardized actionable metrics per cohort; pivot/persevere gates each quarter.

**Experiment backlog (test cards)** — examples:

- **Problem:** Missed calls at clinics (Spain). **Riskiest assumption:** Patients accept voice bot for scheduling. **Metric:** Booking completion rate. **N:** 300 calls. **Time:** 2 weeks. **Rule:** Launch if ≥35% bookings complete end-to-end; else add human fallback earlier.  
- **Problem:** WER on noisy mobile lines (MX). **Assumption:** Domain grammar + confirmation reduces errors. **Metric:** WER; *A/B* baseline vs grammar prompts. **N:** 200 calls. **Rule:** Promote if WER ↓ ≥25%.  
- **Problem:** Consent prompt friction (CL). **Assumption:** Inline consent phrasing reduces hang-ups. **Metric:** Drop-off in first 10s. **N:** 150 calls. **Rule:** Keep variant with ≤3% drop-off.

---

## 4) OKR program (Measure What Matters)

**Company North-Star Objective (annual):**  
*Delight callers with instant, accurate assistance while eliminating missed calls.*  
**KRs:** (1) Containment ≥60% Spain; ≥55% LATAM by Q4. (2) p95 connect ≤3s; intra-turn <250ms. (3) CSAT proxy ≥4.3/5. (4) Revenue run-rate ≥€1.2M ARR. (5) DSR turnaround ≤7 days, 100% on-time.

**Quarterly OKRs (sample)**

- **Product**  
  - KR1: Median handle time 4–6 min; KR2: Tool-call error rate <1%; KR3: NLU intent accuracy ≥92%.  
- **Engineering/SRE**  
  - KR1: 99.7% avail (Q2) → 99.9% (Q3) SLO; KR2: p95 end-to-end latency ≤800ms; KR3: zero Sev-1 from data leakage; KR4: load: 200 concurrent sustained for 30 min without breach.  
- **Compliance**  
  - KR1: 100% calls play jurisdictional consent; KR2: DPA/SCCs signed with vendors; KR3: DSR SLA ≤7 days; KR4: quarterly access reviews completed.  
- **GTM**  
  - KR1: 20 pilot logos Spain; KR2: 10 paid conversions; KR3: NRR ≥110%; KR4: win-rate vs manual answering ≥40%.

**Cadence:** Quarterly planning; **weekly check-ins** with confidence scores; mid-quarter re-basing; postmortems per objective.

**Example OKR tree:** Company → Product (Voice UX) → Squads: *(n8n Workflow)*, *(Telephony/Voice)*, *(Compliance/Governance)*.

---

## 5) Technical architecture (n8n + Retell AI)

### (a) End-to-end call flow (ASCII)

```
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
```

### (b) Data flow & storage classification (ASCII)

```
[Audio RTP/WebRTC] (transient) -> Retell streaming
[Partial transcripts] (ephemeral buffer) -> redaction pipeline -> stored per retention
[Tool payloads] (PII-minimized) -> n8n queue -> vendor APIs
[Metadata] (session IDs, timings) -> metrics TSDB
[Recordings] (optional, dual-channel) -> CPaaS storage (encrypted) w/ retention policy
```

### (c) High-availability layout (ASCII)

```
Multi-Region Ingress: EU-West (Spain) + US-East + LATAM (via CPaaS PoPs)
         |
   Stateless Retell sessions (auto-scale) ── Circuit Breakers ── DLQ (tool calls)
         |
     n8n cluster (queue mode + Redis/RabbitMQ + Postgres HA)
         |
   Observability stack (OpenTelemetry -> log store + metrics + traces)
```

**Responsibilities & SLAs**  
- **Retell AI:** Realtime ASR/LLM/TTS; barge-in; session control; webhooks; PII redaction; configurable storage. *SLO:* sub-second first token; sub-300ms turn latency; GDPR/HIPAA posture.  
- **n8n:** Tool execution, state, retries/idempotency, orchestration; queue mode horizontal scaling; webhook trigger.  
- **CPaaS (Twilio/Infobip/SIP):** Numbers, PSTN bridging, call recording, warm transfer.

### Sequence (barge-in + tool call with budgets)

```
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
```

---

## 6) Detailed Retell AI integration plan

**Sessions & tools**  
- **Create session:** choose voice (ES-ES, ES-MX variants), temperature, latency preset, interruption settings (barge-in on, stop-on-voice).  
- **Tools schema:** declarative “functions” with JSON schema; Retell triggers **`tool_call`** → n8n executes and replies with **`tool_result`**.  
- **n8n role:** tool executor/state machine; keeps conversation state and business invariants.

**Webhooks & signed callbacks**  
- Use Retell **webhooks** for `session_started`, `tool_call`, `transfer_to_human`, `session_ended`. Include **HMAC signature** (shared secret) and **Idempotency-Key** header and correlation IDs. Retries: exponential backoff; accept-once semantics.

**Sample payloads (abridged JSON)**

- `session_started`  
```json
{
  "event": "session_started",
  "session_id": "rt-123",
  "caller": { "ani": "+3491XXXXXXX", "locale": "es-ES" },
  "consent": { "jurisdiction": "ES", "recording_enabled": true }
}
```

- `tool_call`  
```json
{
  "event": "tool_call",
  "session_id": "rt-123",
  "tool": "create_calendar_event",
  "args": { "name": "María Gómez", "when": "2025-10-22T10:00:00+02:00", "channel":"phone" }
}
```

- n8n returns `tool_result`  
```json
{
  "session_id": "rt-123",
  "tool": "create_calendar_event",
  "result": { "status": "ok", "calendar_id":"A-1", "event_id":"evt_987", "join":"tel:+3491...." }
}
```

- `transfer_to_human`  
```json
{
  "event": "transfer_to_human",
  "session_id": "rt-123",
  "target": { "sip":"sip:frontdesk@pbx.example.com" },
  "whisper": { "intent":"booking", "summary":"Cita mañana 10:00, María Gómez", "crm_id":"lead_2025-10-21_001" }
}
```

- `session_ended`  
```json
{ "event":"session_ended","session_id":"rt-123","reason":"caller_hangup","dur_s":362 }
```

**n8n artifacts**

- **Nodes (core):** *Webhook (Retell in)* → *Function (HMAC verify + route)* → *Switch (event type)* → *Tool workers (HTTP Request, Google/Microsoft Calendar, CRM API, Payments/Tickets, Twilio SMS/WhatsApp)* → *Return (tool_result)* → *Postgres/BigQuery log* → *Metrics (HTTP)*.  
- **Workflow (a) Intent→Schedule→CRM note (pseudo-JSON):**
```json
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
```
- **Workflow (b) Lead capture → qualification → warm transfer + whisper:** same pattern; resolve human target via schedule/queue, send whisper context, then `transfer_to_human`.  
- **Workflow (c) Voicemail fallback:** on busy/no agents, branch to CPaaS voicemail; transcript via Retell/CPaaS transcription; follow-up via Twilio SMS/WhatsApp or email.

**Localization**  
- Prompt packs per variant (**ES-ES, ES-MX, ES-CO, ES-AR, ES-CL, ES-PE**), with *slot-filling* for names, addresses, and national IDs (NIF/NIE; CURP; RUT/RUN; CUIL/CUIT; DNI). Pronunciation and disambiguation dictionaries maintained in repo.

---

## 7) Telephony & call operations (Spain & LATAM)

- **Numbers & routing:** Use Twilio (and/or Infobip, or direct SIP trunks).  
- **Consent/recording prompts:** Always announce recording & purpose; store consent flag. Country checklists in Appendix.  
- **DTMF fallback & dual-channel recording:** Enable DTMF paths for noisy lines; dual-channel recordings for QA and redaction.  
- **Jitter/packet loss:** Prefer Opus @ 16kHz; use jitter buffers; failover to PSTN bridge if QoS < threshold.  
- **Warm transfer:** Whisper to human (“*Transferencia caliente: María desea cita mañana 10:00*”), pass context via SIP headers/CRM link; SLA: p95 <5s.  
- **Concurrency:** Size Retell sessions & n8n workers for peaks: 5 / 50 / 200 concurrent; use queue mode autoscaling and backpressure (details §9).

---

## 8) Security, privacy, and compliance blueprint

- **Data minimization & PII:** Redact at source (Retell PII redaction), store only business-necessary fields; encryption in transit (TLS) & at rest; KMS-managed keys.  
- **GDPR/LOPDGDD in Spain:** Informed consent on first utterance; DSRs: access/erasure/portability via n8n; audit log of disclosures. Retell privacy/controls & GDPR statements.  
- **LATAM regimes (high level):** Mexico (LFPDPPP/INAI), Colombia (Law 1581/SIC), Argentina (Law 25.326/AAIP), Chile (reform underway), Peru (Law 29733). Use conservative consent + local data transfer notices; SCCs for cross-border if EU data accessed.  
- **Access model:** SSO (IdP), RBAC least privilege, quarterly access reviews, secret rotation; **DPIA** per customer vertical.

---

## 9) Performance engineering & SRE

**Latency budget (targets)**  
- Call setup (PSTN→agent): 0.6–1.2s  
- First token (ASR→LLM→TTS): ≤1.2s  
- Barge-in stop + turn latency: <300ms  
- Tool round-trip (n8n + API): typical 150–600ms; budget spikes to 1.2s with retries.

**Autoscaling & backpressure**  
- Retell sessions: horizontal autoscale by active streams & CPU; shed load to “please hold” prompt if saturation.  
- n8n queue workers: autoscale by queue depth/age; circuit-break slow tools; DLQ poison messages.  
- Degrade: switch to DTMF or voicemail when external APIs exceed SLOs.

**Reliability patterns**  
- Retries with exponential *jittered* backoff; idempotency keys (session_id + event_id).  
- Circuit breakers around CRM/calendar; bulkheads per integration.  
- DLQs with replay; rate-limiters per vendor.

**Observability**  
- Structured JSON logs incl. correlation IDs (`cp-call-id`, `retell-session-id`, `n8n-exec-id`).  
- OpenTelemetry traces across CPaaS → Retell → n8n → API; p50/p95 dashboards; redaction at log sink.  
- Session replays using redacted transcripts + dual-channel audio.

---

## 10) Cost model & financial plan

**Assumptions:**  
- **FX:** 1 EUR = 1.1655 USD (ECB, Oct 20, 2025).  
- **Retell AI:** usage-based per minute (starter/business ranges).  
- **Twilio Voice (reference):** Recording €0.0025/min; Storage €0.0005/min-month, first 10k min storage free; voice rates vary by country.  
- **n8n Cloud:** pricing by executions (or self-host).  

> **Note:** Country voice rates (inbound/outbound) differ. Use CPaaS country pages to plug exact destinations for Spain & each LATAM country; below uses blended ranges.

### Unit BOM (per conversation minute)

| Component | Low (€/min) | Base (€/min) | High (€/min) | Notes |
|---|---:|---:|---:|---|
| Retell AI agent minutes | 0.050 | 0.080 | 0.120 | Tier-dependent |
| Telephony (avg blended 80/20 in/out) | 0.012 | 0.020 | 0.035 | Country mix |
| Recording (if on) | 0.0025 | 0.0025 | 0.0025 | |
| Storage (avg 1 month) | 0.0005 | 0.0005 | 0.0005 | |
| n8n Cloud executions | 0.002 | 0.004 | 0.008 | Tool-calls/min |
| **Total €/min** | **0.067** | **0.107** | **0.166** | – |

**Token & speech assumptions:** 140–180 wpm; ~3 chars/token; TTS ~1.1× tokens of user speech; silence trimming reduces TTS 10–15%.

### Monthly costs @ 80/20 in/out; minutes = 100/1k/10k/100k

| Minutes/mo | Low (€) | Base (€) | High (€) |
|---:|---:|---:|---:|
| 100 | 6.7 | 10.7 | 16.6 |
| 1,000 | 67 | 107 | 166 |
| 10,000 | 670 | 1,070 | 1,660 |
| 100,000 | 6,700 | 10,700 | 16,600 |

*(Add phone numbers: €1–€5 per number/month; plus support & compliance overhead.)*

**Sensitivity**

- **A) Call length (Base €/min = 0.107)**

| Minutes | 2 min | 4 min | 8 min | 12 min |
|---:|---:|---:|---:|---:|
| 1,000 | €214 | €428 | €856 | €1,284 |
| 10,000 | €2,140 | €4,280 | €8,560 | €12,840 |

- **B) Model tier swap (illustrative)**  
  - Fast model: Retell €/min ↓ ~20% → Base ≈ €0.091/min.  
  - Flagship model: Retell €/min ↑ ~25% → Base ≈ €0.134/min.  
  - ASR/TTS à-la-carte fallback (Deepgram/ElevenLabs) can adjust cost by a few €cents/minute depending on talk ratio.

**One-time & monthly overhead**  
- **One-time:** Discovery, scripting, prompts, workflow build, telephony config, DPIA: **€6k–€25k** per brand.  
- **Monthly:** Observability, number rental, compliance ops, support tiers: **€200–€1,500** per tenant.

*(Formulas:* **Cost = Minutes × €/min + Fixed**; **€/min** = sum(Unit BOM))*

---

## 11) Localization, voice UX & conversation design

- **Personas:** *Recepcionista cordial* (Spain) / *Asistente amable* (LATAM) with country-specific politeness and code-switching (ES↔EN).  
- **Guardrails:** No advice outside domain; confirm sensitive operations (payments); repeat back IDs.  
- **Grammar packs:** Names (ñ/accents), addresses; IDs: Spain NIF/NIE; MX CURP/RFC; CO NIT/CC; AR CUIT/CUIL/DNI; CL RUT; PE DNI/RUC.  
- **Disambiguation & error handling:** “¿Podría deletrear su apellido, por favor?”; top-two confirmation strategy; background noise prompts.  
- **Test matrix:** Noisy line SNR 5–15 dB; crosstalk; slang (chido/chévere/bacán/pata); accents (castellano, andaluz; MX norte/centro; CO paisa/costeño; AR rioplatense; CL chileno; PE limeño).

---

## 12) Rollout plan & timelines

**Spain 90-day MVP**

| Phase | Weeks | Exit criteria |
|---|---:|---|
| Discovery & prototype | 1–4 | Consent scripts validated; baseline WER test; two core workflows in staging; p95 FR ≤1.4s |
| Pilot (10–15 logos) | 5–8 | Containment ≥45%; transfer p95 <5s; no Sev-1 incidents; DSR flow proven |
| Beta (broader) | 9–12 | Containment ≥50%; CSAT ≥4.2/5; 99.5% avail; go for paid rollout |

**LATAM expansion order & key tasks**  
- **Peru → Mexico → Colombia → Chile → Argentina** (telecom + compliance readiness, WhatsApp opt-ins, local prompts; numbers & consent scripts).  
- **Gantt milestones:** Number procurement, Retell environment, n8n workflows, consent QA, pilot, production cutover (blue/green), canary 10% → 50% → 100%, rollback runbook.

---

## 13) Testing & QA

- **Automated:** Synthetic callers generate 2/4/8/12-min conversations; capture p50/p95 latencies, barge-in success, transfer success; WER by accent; function-call error rate.  
- **Manual:** Golden dialogs per country; rubric scoring (intent, accuracy, empathy, compliance).  
- **Security & compliance:** Consent prompt detection; retention checks; access review evidence; DSR request test.

---

## 14) GTM & customer success

- **Pilot criteria:** 300–1,500 mins/mo; clear FAQs; calendar access; one responsible owner; willingness to co-design prompts.  
- **Onboarding playbook:** Numbers, consent, calendars/CRM, training, go-live checklist.  
- **Pricing one-pager & ROI calculator:** Savings vs missed calls and receptionist coverage.  
- **Support & SLAs:** Tiered (Standard 8×5; Plus 12×5; Premium 24×5). Escalation matrix; incident runbooks.  
- **Feedback → backlog:** Weekly OKR check-ins; defects triage; experiment proposals.

---

## 15) Risk register & mitigations (sample of 15)

| Risk | Likelihood | Impact | Owner | Mitigation |
|---|---:|---:|---|---|
| Latency spikes at 200 concurrent | M | H | SRE | Autoscale triggers; pre-warmed pools; degrade to DTMF |
| High WER on accents/noise | M | M | Voice Eng | Grammar packs; confirmations; targeted retraining |
| Consent/recording non-compliance | L | H | Compliance | Jurisdiction prompts; audits; legal review |
| Vendor outage (CPaaS/Retell) | L | H | SRE | Multi-region; failover trunks; clear RCAs |
| Tool API rate limits | M | M | n8n Dev | Caching; rate limiter; CQRS + queues |
| Data leak via logs | L | H | SecOps | Redaction on ingest; PII scanning; tokenization |
| Calendar double-booking | M | M | Product | Optimistic concurrency; refresh/confirm |
| Warm transfer fails | M | M | Telephony | SIP fallback; voicemail branch |
| Payment lookup API changes | M | M | n8n Dev | Contract tests; versioned connectors |
| Regulatory changes (country) | M | M | Compliance | Country checklists; legal counsel; feature flags |
| Cost overrun (telephony) | M | M | Finance | Alerting/thresholds; route optimization |
| Customer prompt drift | M | M | PM | Prompt QA; versioning; A/B |
| Security breach | L | H | CISO | SSO/RBAC; pentests; rotation |
| Lock-in to a single vendor | M | H | Arch | Abstraction over tools; export formats; “switch day” runbook |
| Negative UX (robotic voice) | M | M | UX | Premium voices; prosody tuning; barge-in tuning |

**Lock-in strategy:** All business logic/tools live in **n8n** with clean contracts; Retell functions mirror a vendor-neutral schema; recordings/transcripts exported periodically; runbook for swapping CPaaS or voice agent with 1-week dry run.

---

## 16) Appendices

### A) Glossary (telephony/voice AI)  
*Barge-in, dual-channel recording, DLQ, SLO/SLA/SLA breach, DSR, DPIA, CQRS, etc.*

### B) Assumptions & formulas  
- **€/min** = sum(Unit BOM rows). **Monthly** = minutes × €/min + fixed. **FX** at ECB rate on **2025-10-20**.

### C) Example n8n workflow & Retell config (snippets)

**Retell session config (ES-ES / fast latency)**  
```json
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
```

**n8n webhook verifier (pseudo):**  
```js
if (!verifyHmac($json.headers, $binary.raw)) return $respond(401);
routeBy($json.event);
```

### D) Country legal checklists (concise)

- **Spain (GDPR/LOPDGDD):** Inform of recording & purpose; allow no-recording path if requested; DSRs: access, deletion, portability via n8n; DPA with vendors; SCCs if exporting data outside EEA.  
- **Mexico:** LFPDPPP/INAI — privacy notice, consent for processing/transfer; rights ARCO; keep call purposes explicit.  
- **Colombia:** Law 1581/2012 — authorization & database registration if applicable; SIC guidance.  
- **Argentina:** Law 25.326 — prior consent; international transfer rules (AAIP).  
- **Chile:** Current law + reform bill — adopt conservative consent practice.  
- **Peru:** Law 29733 — consent; ANPDP guidance.

### E) Launch checklists

**Spain (concise):**  
1) Numbers & routing; 2) Consent scripts (ES-ES); 3) Retell session presets; 4) n8n workflows (FAQ, scheduling, CRM); 5) DSR runbooks; 6) Observability dashboards; 7) Pilot UAT & go-live; 8) Canary & rollback plan.

**LATAM reusable:**  
1) Country taxes & billing; 2) Local consent wording; 3) Numbers & WhatsApp templates; 4) Prompt localization; 5) Legal DPIA & DPA/SCCs; 6) Pilot cohort & QA; 7) Canaries; 8) Support hours local TZ.

---

## Tying it all together (Lean + OKRs)

- Every phase closes with a **go/no-go** based on actionable metrics within innovation accounting (containment, latency, WER, CSAT proxy, transfer success, compliance SLAs).  
- The OKR program provides the **operating rhythm** (weekly check-ins, mid-quarter re-basing, postmortems) to ensure we **pivot or persevere** deliberately.

**Próximo paso:** Start Spain week-1 discovery with two lighthouse clinics and one home-services brand; set up sandbox experiments before the week-5 pilot.
