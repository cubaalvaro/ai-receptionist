# 12–13) Rollout plan, testing & QA

Spain 90-day MVP, LATAM expansion, QA matrices.

12) Rollout plan & timelines
Spain 90-day MVP

Phase	Weeks	Exit criteria
Discovery & prototype	1–4	Consent scripts validated; baseline WER test; two core workflows in staging; p95 FR ≤1.4s
Pilot (10–15 logos)	5–8	Containment ≥45%; transfer p95 <5s; no Sev-1 incidents; DSR flow proven
Beta (broader)	9–12	Containment ≥50%; CSAT ≥4.2/5; 99.5% avail; go for paid rollout
LATAM expansion order & key tasks

Peru → Mexico → Colombia → Chile → Argentina (telecom + compliance readiness, WhatsApp opt-ins, local prompts; numbers & consent scripts).
Gantt milestones: Number procurement, Retell environment, n8n workflows, consent QA, pilot, production cutover (blue/green), canary 10% → 50% → 100%, rollback runbook.

13) Testing & QA
Automated: Synthetic callers generate 2/4/8/12-min conversations; capture p50/p95 latencies, barge-in success, transfer success; WER by accent; function-call error rate.
Manual: Golden dialogs per country; rubric scoring (intent, accuracy, empathy, compliance).
Security & compliance: Consent prompt detection; retention checks; access review evidence; DSR request test.
