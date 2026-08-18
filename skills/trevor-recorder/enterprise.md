# Enterprise bar (Trevor-owned)

- Secrets: env + Secrets Manager. Never commit `.env`, keys, Bedrock creds, Cognito secrets.
- AWS auth: GitHub OIDC. No `AKIA` in Actions.
- Tenants: two demo keys (`tenant-a`, `tenant-b`). SDK sends `tenant_id` on every span.
- Prompts: Alexis redacts at the door. Prefer client-side hash when marked sensitive. **Never log raw prompts.**
- Network: TLS, WAF on API Gateway, no public SSH `:22`, no public S3 list.
- Data plane in Terraform: S3 SSE-KMS, DynamoDB encryption, TTL 7d, IAM least privilege. No `*` on `s3:` or `dynamodb:` except a documented exception.
- CI: `gitleaks`, `trivy fs`, `bandit` on Python, `terraform plan` on `infra/**`. Deploy only from `main`.
- No Grafana as product UI. No Langfuse as store. No EKS/AKS.

Kill order if time slips: extra span kinds → extra Terraform niceties. Never kill: schema-valid emit, two tenants, demo script, OIDC/CI, live URL.
