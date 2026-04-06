# MsgVet — Hard Rules

## Data & Privacy
1. Every database query MUST include tenantId filter. No cross-tenant data access.
2. Kid data (under-13) follows COPPA: verifiable parental consent, minimal retention, no ad targeting.
3. Credentials and secrets NEVER stored in database. Key Vault references only.
4. Alias labels encrypted with user-specific keys. Server cannot read them.
5. All message content encrypted at rest (AES-256, per-tenant keys).

## Vetting Pipeline
6. No outbound message bypasses the vet pipeline. Emergency bypass requires biometric + audit flag.
7. Every inbound message to a kid account runs the full filtering pipeline. No exceptions.
8. Deterministic rules (allow/deny lists, regex, time windows) execute BEFORE any LLM call.
9. Self-harm and threat signals ALWAYS escalate to strong model AND trigger CRITICAL parent alert.
10. LLM prompt injection in message content must be sanitized. No tool-use in vet pipeline.

## Audit
11. Every state change produces an audit event with hash chain integrity.
12. Audit events are append-only. No updates or deletes.
13. Every send operation uses an idempotency key.

## Architecture
14. Services communicate via Service Bus for async operations, REST for sync.
15. All list APIs support cursor pagination with allowed sizes: 25, 50, 100, 500, 1000.
16. API versioning via URL path prefix (/api/v1/).

## Cost
17. LLM calls use cheap model by default. Escalate to strong model only on low confidence or policy trigger.
18. Rewrite suggestions generated on-demand only (not pre-computed for every message).
