# Workflow

1. Read `PLAN.md`.
2. Read `skills/trevor-recorder/SKILL.md`.
3. Read **one** `agents/*.md` matching your id.
4. Lease paths. Stop on overlap.
5. Implement only those paths. Tests first on SDK.
6. No `terraform apply`, no servers, no commit unless the human asked.
7. Handoff note if blocked.
8. Release lease.

## Done (lane)

- [ ] SDK golden + nesting + no-raw-log tests
- [ ] Demo fake-bedrock span graph test
- [ ] `scripts/demo_pii_flight.sh`
- [ ] Terraform validate; plan has Cognito, KMS, WAF, TTL, OIDC
- [ ] gitleaks + trivy workflows
- [ ] Zero files under `vault/` or `web/` from Trevor
- [ ] Human-approved apply then URL kept alive
