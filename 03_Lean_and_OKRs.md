# 3–4) Lean Startup strategy & OKR program

3) Lean Startup
Contains Build–Measure–Learn loops, hypotheses, and quarterly OKRs.

Horizons & falsifiable hypotheses

Horizon	Weeks	Example hypotheses	Success criteria (falsifiable)
Pre-MVP	0–4	H1: ≥50% of inbound intents (routing/FAQ/scheduling) can be handled with ≤1.2s first response; H2: Users accept consent prompts without drop-off >5%	p95 FR ≤1.2s; barge-in latency <300ms; consent drop-off ≤5%; WER ≤10%
Beta	5–12	H3: Containment ≥50% at 4–6 min avg handle; H4: Warm transfers succeed ≥95% with <5s time-to-agent	Containment ≥50%; transfer success ≥95%; CSAT proxy ≥4.2/5
Scale	4–12 mo	H5: Gross margin ≥65% at 10k–100k mins; H6: 99.9% avail with multi-region ingress	GM ≥65%; SLO: 99.9%; p95 intra-turn <250ms
Innovation accounting & cohorting: Standardized actionable metrics per cohort; pivot/persevere gates each quarter.

Experiment backlog (test cards) — examples:

Problem: Missed calls at clinics (Spain). Riskiest assumption: Patients accept voice bot for scheduling. Metric: Booking completion rate. N: 300 calls. Time: 2 weeks. Rule: Launch if ≥35% bookings complete end-to-end; else add human fallback earlier.
Problem: WER on noisy mobile lines (MX). Assumption: Domain grammar + confirmation reduces errors. Metric: WER; A/B baseline vs grammar prompts. N: 200 calls. Rule: Promote if WER ↓ ≥25%.
Problem: Consent prompt friction (CL). Assumption: Inline consent phrasing reduces hang-ups. Metric: Drop-off in first 10s. N: 150 calls. Rule: Keep variant with ≤3% drop-off.

4) OKR program (Measure What Matters)
Company North-Star Objective (annual):
Delight callers with instant, accurate assistance while eliminating missed calls.
KRs: (1) Containment ≥60% Spain; ≥55% LATAM by Q4. (2) p95 connect ≤3s; intra-turn <250ms. (3) CSAT proxy ≥4.3/5. (4) Revenue run-rate ≥€1.2M ARR. (5) DSR turnaround ≤7 days, 100% on-time.

Quarterly OKRs (sample)

Product
KR1: Median handle time 4–6 min; KR2: Tool-call error rate <1%; KR3: NLU intent accuracy ≥92%.
Engineering/SRE
KR1: 99.7% avail (Q2) → 99.9% (Q3) SLO; KR2: p95 end-to-end latency ≤800ms; KR3: zero Sev-1 from data leakage; KR4: load: 200 concurrent sustained for 30 min without breach.
Compliance
KR1: 100% calls play jurisdictional consent; KR2: DPA/SCCs signed with vendors; KR3: DSR SLA ≤7 days; KR4: quarterly access reviews completed.
GTM
KR1: 20 pilot logos Spain; KR2: 10 paid conversions; KR3: NRR ≥110%; KR4: win-rate vs manual answering ≥40%.
Cadence: Quarterly planning; weekly check-ins with confidence scores; mid-quarter re-basing; postmortems per objective.

Example OKR tree: Company → Product (Voice UX) → Squads: (n8n Workflow), (Telephony/Voice), (Compliance/Governance).
