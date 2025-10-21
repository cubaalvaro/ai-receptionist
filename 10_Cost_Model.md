# 10) Cost model & financial plan

Includes per-minute unit economics, sensitivity tables, and assumptions.

Assumptions:

FX: 1 EUR = 1.1655 USD (ECB, Oct 20, 2025).
Retell AI: usage-based per minute (starter/business ranges).
Twilio Voice (reference): Recording €0.0025/min; Storage €0.0005/min-month, first 10k min storage free; voice rates vary by country.
n8n Cloud: pricing by executions (or self-host).
Note: Country voice rates (inbound/outbound) differ. Use CPaaS country pages to plug exact destinations for Spain & each LATAM country; below uses blended ranges.

Unit BOM (per conversation minute)
Component	Low (€/min)	Base (€/min)	High (€/min)	Notes
Retell AI agent minutes	0.050	0.080	0.120	Tier-dependent
Telephony (avg blended 80/20 in/out)	0.012	0.020	0.035	Country mix
Recording (if on)	0.0025	0.0025	0.0025	
Storage (avg 1 month)	0.0005	0.0005	0.0005	
n8n Cloud executions	0.002	0.004	0.008	Tool-calls/min
Total €/min	0.067	0.107	0.166	–
Token & speech assumptions: 140–180 wpm; ~3 chars/token; TTS ~1.1× tokens of user speech; silence trimming reduces TTS 10–15%.

Monthly costs @ 80/20 in/out; minutes = 100/1k/10k/100k
Minutes/mo	Low (€)	Base (€)	High (€)
100	6.7	10.7	16.6
1,000	67	107	166
10,000	670	1,070	1,660
100,000	6,700	10,700	16,600
(Add phone numbers: €1–€5 per number/month; plus support & compliance overhead.)

Sensitivity

A) Call length (Base €/min = 0.107)
Minutes	2 min	4 min	8 min	12 min
1,000	€214	€428	€856	€1,284
10,000	€2,140	€4,280	€8,560	€12,840
B) Model tier swap (illustrative)
Fast model: Retell €/min ↓ ~20% → Base ≈ €0.091/min.
Flagship model: Retell €/min ↑ ~25% → Base ≈ €0.134/min.
ASR/TTS à-la-carte fallback (Deepgram/ElevenLabs) can adjust cost by a few €cents/minute depending on talk ratio.
One-time & monthly overhead

One-time: Discovery, scripting, prompts, workflow build, telephony config, DPIA: €6k–€25k per brand.
Monthly: Observability, number rental, compliance ops, support tiers: €200–€1,500 per tenant.
(Formulas: Cost = Minutes × €/min + Fixed; €/min = sum(Unit BOM))*
