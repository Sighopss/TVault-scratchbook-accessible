# Enterprise bar

Trevor’s slice of “observability must not become a data-leakage mechanism.”

- No secrets in git. Secrets Manager + env. `.env` gitignored.
- GitHub Actions authenticates with **OIDC**. If you are about to paste `AKIA...`, stop.
- Two tenants in the demo. Every span has `tenant_id`. Wrong tenant is Alexis’s 403, but Trevor must not mix keys.
- Never log raw prompts. `sensitive=True` hashes and masks before any logger.
- TLS everywhere. WAF on the HTTP API. CORS origin = CloudFront only. No SSH `:22` to `0.0.0.0/0`. S3 public access block.
- KMS on payload bucket and table. DynamoDB TTL 7 days (`expires_at`). PITR off. CloudTrail off.
- Remote Terraform state (S3 + lock). Lambda logs 7d. API 5xx alarm. `GET /health` mock.
- IAM: no `Action = "*"` on `s3` or `dynamodb` without a comment the human accepted.
- CI: gitleaks, trivy, bandit, vault pytest, web Playwright, terraform validate. Deploy **main only**. Rollback = re-run last green deploy.
- Grafana is not a deliverable. InnerAI already did “wrap the LLM and dump logs.” We do not.

Time-slip cuts: extra span kinds, CloudTrail, SNS pager, custom domain, RCA Bedrock. Never cut: schema-valid emit, two tenants, JWT→`custom:tenant_id`, CORS, `/health`, demo script, OIDC, public URL after approved apply.
