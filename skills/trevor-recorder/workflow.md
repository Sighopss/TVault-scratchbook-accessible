# Workflow

1. Read `PLAN.md`.
2. Read `skills/trevor-recorder/SKILL.md`.
3. Read **one** `agents/*.md` matching your id.
4. Lease paths **and** open PRs (`handoffs/README.md`). Stop on overlap.
5. Implement only those paths. Tests first on SDK. Write `handoffs/trevor-<id>-<slug>.md` on this branch.
6. No `terraform apply`, no servers, no commit unless the human asked.
7. PR body = that handoff. Do not merge — Trevor merges.
8. Release lease.

## Done (lane)

- [ ] SDK golden + nesting + no-raw-log tests
- [ ] Demo fake-bedrock span graph test
- [ ] `scripts/demo_pii_flight.sh`
- [ ] Terraform validate; plan has Cognito `custom:tenant_id`, two Lambdas, CORS, `/health`, KMS, WAF, TTL, OIDC, alarm
- [ ] gitleaks + trivy + vault.yml + web.yml + deploy sync
- [ ] Zero files under `vault/` or `web/` from Trevor
- [ ] Human-approved apply then URL kept alive
