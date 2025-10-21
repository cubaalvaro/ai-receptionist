# 8) Security, privacy, and compliance blueprint

GDPR/LOPDGDD baseline, LATAM privacy laws, RBAC, DPIA, encryption.

Data minimization & PII: Redact at source (Retell PII redaction), store only business-necessary fields; encryption in transit (TLS) & at rest; KMS-managed keys.
GDPR/LOPDGDD in Spain: Informed consent on first utterance; DSRs: access/erasure/portability via n8n; audit log of disclosures. Retell privacy/controls & GDPR statements.
LATAM regimes (high level): Mexico (LFPDPPP/INAI), Colombia (Law 1581/SIC), Argentina (Law 25.326/AAIP), Chile (reform underway), Peru (Law 29733). Use conservative consent + local data transfer notices; SCCs for cross-border if EU data accessed.
Access model: SSO (IdP), RBAC least privilege, quarterly access reviews, secret rotation; DPIA per customer vertical.
