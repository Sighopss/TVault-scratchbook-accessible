# Workflow

1. Read `PLAN.md`, then `skills/trevor-recorder/SKILL.md`, then `contracts/span.schema.json`.
2. Claim a lease ([parallel.md](parallel.md)). Refuse overlap.
3. Work only in leased paths ([ownership.md](ownership.md)).
4. Tests first for SDK emit.
5. Do not `terraform apply` or start servers unless the human asked.
6. Stop at lane boundaries. Note URL, env names, fixture path ([handoffs.md](handoffs.md)).
7. Release the lease.

## Done

- [ ] Schema exists; SDK golden test passes
- [ ] Demo emits one multi-span flight
- [ ] `demo_pii_flight.sh` is the judge trigger
- [ ] Terraform plan: Cognito two users, KMS, WAF, TTL, OIDC-ready
- [ ] CI fails on secrets; deploys only `main`
- [ ] No writes under `vault/` or `web/`
- [ ] Public URL kept alive after first human-approved apply
