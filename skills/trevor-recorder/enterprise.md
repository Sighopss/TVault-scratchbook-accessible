# Enterprise bar

Trevor’s slice of “observability must not become a data-leakage mechanism.”

- No secrets in git. Secrets Manager + env. `.env` gitignored.
- GitHub Actions authenticates with **OIDC**. If you are about to paste `AKIA...`, stop.
- Two tenants in the demo. Every span has `tenant_id`. Wrong tenant is Alexis’s 403, but Trevor must not mix keys.
- Never log raw prompts. `sensitive=True` hashes and masks before any logger.
- TLS everywhere. WAF on the HTTP API. No SSH `:22` to `0.0.0.0/0`. S3 public access block.
- KMS on payload bucket and table. DynamoDB TTL 7 days (`expires_at`).
- IAM: no `Action = "*"` on `s3` or `dynamodb` without a comment the human accepted.
- CI: gitleaks, trivy, bandit, terraform validate. Deploy **main only**.
- Grafana is not a deliverable. InnerAI already did “wrap the LLM and dump logs.” We do not.

Time-slip cuts: extra span kinds, CloudTrail, fancy dashboards in AWS console. Never cut: schema-valid emit, two tenants, demo script, OIDC, public URL after approved apply.
