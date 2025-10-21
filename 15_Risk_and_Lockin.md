# 15) Risk register & mitigations

Includes vendor lock-in strategy and 15 risk entries.

Risk	Likelihood	Impact	Owner	Mitigation
Latency spikes at 200 concurrent	M	H	SRE	Autoscale triggers; pre-warmed pools; degrade to DTMF
High WER on accents/noise	M	M	Voice Eng	Grammar packs; confirmations; targeted retraining
Consent/recording non-compliance	L	H	Compliance	Jurisdiction prompts; audits; legal review
Vendor outage (CPaaS/Retell)	L	H	SRE	Multi-region; failover trunks; clear RCAs
Tool API rate limits	M	M	n8n Dev	Caching; rate limiter; CQRS + queues
Data leak via logs	L	H	SecOps	Redaction on ingest; PII scanning; tokenization
Calendar double-booking	M	M	Product	Optimistic concurrency; refresh/confirm
Warm transfer fails	M	M	Telephony	SIP fallback; voicemail branch
Payment lookup API changes	M	M	n8n Dev	Contract tests; versioned connectors
Regulatory changes (country)	M	M	Compliance	Country checklists; legal counsel; feature flags
Cost overrun (telephony)	M	M	Finance	Alerting/thresholds; route optimization
Customer prompt drift	M	M	PM	Prompt QA; versioning; A/B
Security breach	L	H	CISO	SSO/RBAC; pentests; rotation
Lock-in to a single vendor	M	H	Arch	Abstraction over tools; export formats; “switch day” runbook
Negative UX (robotic voice)	M	M	UX	Premium voices; prosody tuning; barge-in tuning
Lock-in strategy: All business logic/tools live in n8n with clean contracts; Retell functions mirror a vendor-neutral schema; recordings/transcripts exported periodically; runbook for swapping CPaaS or voice agent with 1-week dry run.
