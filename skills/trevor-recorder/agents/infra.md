# Mission: trevor-infra

**Id:** `trevor-infra`  
**Write:** `infra/` only  
**Do not apply** unless the human said `terraform apply`.

Read `PLAN.md` and `contracts/http.draft.md` first. You own AWS so the 48h SaaS bar is real: HTTPS, two tenants, CORS, `/health`, alarm, remote state, two Lambdas, CloudFront that can actually serve Michael’s export.

## Files to create

```
infra/
  versions.tf
  providers.tf
  backend.tf.example   # S3 + DynamoDB lock; human bootstraps once
  variables.tf
  outputs.tf
  main.tf
  kms.tf
  storage.tf           # s3 payloads, s3 web origin, dynamodb + TTL
  api.tf               # HTTP API, CORS, JWT authorizer, API key ingest, /health mock, WAF
  lambda.tf            # vault-ingest + vault-read from ../vault if present else stub zip
  cognito.tf           # pool, app client, hosted UI domain, custom:tenant_id, two users
  cloudfront.tf        # OAC + SPA error routing + response headers (CSP)
  cloudwatch.tf        # log groups 7d, API 5xx alarm
  oidc.tf
  iam.tf
  secrets.tf           # two tenant ingest keys — placeholders, values not in git
  envs/dev.tfvars.example
  envs/prod.tfvars.example
  README.md
```

Terraform >= 1.9. AWS provider ~> 5. Region default `us-east-1`.

## Resources (required)

- Remote state: document S3 bucket + Dynamo lock in README. `backend.tf.example`. Do not commit backend credentials.
- KMS for S3 + DynamoDB
- Payload bucket: SSE-KMS, public access blocked, prefix `{tenant_id}/{trace_id}/`
- Web origin bucket + OAC
- DynamoDB: PK `tenant_id`, SK `trace_id`, TTL `expires_at`, PITR **off**
- HTTP API:
  - CORS allow origin = CloudFront URL (not `*`)
  - `GET /health` mock `{"ok":true}` — no Lambda
  - `POST /v1/traces` → `vault-ingest`, API key / `X-Tenant-Key` (see `contracts/http.draft.md`)
  - `GET /v1/traces`, `GET /v1/traces/{trace_id}`, `GET /v1/traces/{trace_id}/audit` → `vault-read`, Cognito JWT authorizer
  - WAF AWS managed common rule set
- Two Lambdas: `vault-ingest`, `vault-read`. Package `../vault` when that tree exists; otherwise empty stub so `validate` still runs. Env: `TABLE`, `BUCKET`, `KEY_ARN`, secret ARNs. You do not write Python.
- IAM: no `s3:*` on `*`. Ingest can PutObject under tenant prefix. Read can GetItem/Query + GetObject.
- Cognito: user pool, app client, hosted UI domain, callback/logout = CloudFront URL, groups `viewer`/`admin`, users `tenant-a`/`tenant-b` with `custom:tenant_id` matching username. Passwords via `TF_VAR_*` not git.
- CloudFront: default root object, 403/404 → `/index.html` (static export), response headers CSP (default-src self, connect-src API URL).
- GitHub OIDC role limited to this stack
- Secrets Manager: `tenant-a` and `tenant-b` ingest keys (placeholder objects)
- CloudWatch: Lambda log groups retention 7 days. Alarm: API 5xx count ≥ 5 in 5 minutes. SNS optional — skip if no email in tfvars.
- CloudTrail: skip (48h non-goal)

## Outputs

`ingest_url`, `api_url`, `cloudfront_url`, `user_pool_id`, `user_pool_client_id`, `cognito_domain`, `table_name`, `payload_bucket`, `web_bucket`, `oidc_role_arn`

## Checks

`terraform fmt -check`, `terraform validate`. `terraform plan` when creds exist.

## Do not

SSH `:22`. EKS. OpenSearch. Secrets in `*.tf`. Edit `vault/` Python. Apply unprompted. Five Lambdas. Cognito on ingest.

## Done

- [ ] `terraform validate` green
- [ ] README: plan/apply, backend bootstrap, outputs Alexis/Michael need (`NEXT_PUBLIC_*`)
- [ ] Lease released
