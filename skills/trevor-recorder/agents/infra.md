# Mission: trevor-infra

**Id:** `trevor-infra`  
**Write:** `infra/` only  
**Do not apply** unless the human said `terraform apply`.

You own AWS resources so Alexis can hang Lambda code and Michael can hang a CloudFront app.

## Files to create

```
infra/
  versions.tf
  providers.tf
  variables.tf
  outputs.tf
  main.tf
  kms.tf
  storage.tf          # s3 payloads, s3 web origin, dynamodb + TTL
  api.tf              # HTTP API, WAF association
  lambda_stub.tf      # placeholder zip / empty handler
  cognito.tf          # user pool, two users via terraform (passwords from tfvars not git)
  cloudfront.tf
  oidc.tf             # GitHub OIDC provider + role for this repo
  iam.tf
  envs/dev.tfvars.example
  envs/prod.tfvars.example
  README.md
```

Terraform >= 1.9. AWS provider ~> 5. Region variable default `us-east-1`.

## Resources (required)

- KMS key for S3 + DynamoDB
- S3 bucket payloads: SSE-KMS, public access blocked, key prefix idea `{tenant_id}/{trace_id}/`
- DynamoDB: PK `tenant_id`, SK `trace_id`, TTL attribute `expires_at`, PITR off (hackathon)
- HTTP API Gateway + WAF (AWS managed common rule set is enough)
- Lambda stub `vault-ingest` with env `TABLE`, `BUCKET`, `KEY_ARN` — **handler code is Alexis**; you still create the function and IAM
- IAM: Lambda role can PutObject on `arn:...:s3:::bucket/${tenant}` style if possible; never `s3:*` on `*`
- Cognito user pool, app client, groups `viewer` and `admin`, two users `tenant-a` and `tenant-b` (passwords via `TF_VAR_` not committed)
- CloudFront + OAC to empty web bucket
- GitHub OIDC role limited to this stack’s resources
- Secrets Manager placeholders for ingest key (no values in git)
- CloudTrail trail optional; if time-slips, skip trail but keep WAF+KMS+Cognito

## Outputs

`ingest_url`, `cloudfront_url`, `user_pool_id`, `table_name`, `payload_bucket`, `oidc_role_arn`

## Checks

`terraform fmt -check`, `terraform validate`. `terraform plan` when AWS creds exist; if not, still fmt+validate.

## Do not

Open security group SSH 22. Create EKS. Create OpenSearch. Put secrets in `*.tf`. Edit `vault/` Python. Apply unprompted.

## Done

- [ ] `terraform validate` green
- [ ] README: how to plan, which outputs Alexis/Michael need
- [ ] Lease released
