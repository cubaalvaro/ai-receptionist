# 7) Telephony & call operations

Number procurement, routing, transfers, DTMF fallback, concurrency.

Numbers & routing: Use Twilio (and/or Infobip, or direct SIP trunks).
Consent/recording prompts: Always announce recording & purpose; store consent flag. Country checklists in Appendix.
DTMF fallback & dual-channel recording: Enable DTMF paths for noisy lines; dual-channel recordings for QA and redaction.
Jitter/packet loss: Prefer Opus @ 16kHz; use jitter buffers; failover to PSTN bridge if QoS < threshold.
Warm transfer: Whisper to human (“Transferencia caliente: María desea cita mañana 10:00”), pass context via SIP headers/CRM link; SLA: p95 <5s.
Concurrency: Size Retell sessions & n8n workers for peaks: 5 / 50 / 200 concurrent; use queue mode autoscaling and backpressure (details §9).
