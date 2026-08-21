# Handoff — trevor-docs — security-governance

- Date: 2026-08-21
- Human: Trevor
- Agent id: trevor-docs
- Branch: `trevor/docs/security-governance`
- PR: 9
- Mission file: n/a (PLAN)

## Claimed paths (collision)

```
PLAN.md
skills/FILL-CONSTITUTION.md
skills/trevor-recorder/agents/infra.md
skills/trevor-recorder/enterprise.md
handoffs/trevor-docs-security-governance.md
```

## Do not touch

```
vault/
web/
sdk/
demo-app/
infra/
```

## Safe to run in parallel with

Alexis/Michael filling constitutions. Any agent not editing the claimed paths.

## What I shipped

- files: PLAN **Security + governance** (assigned 48h AWS prod bar). Infra/enterprise missions updated.
- outputs / env **names**: none
- tests: none

## What I need

- from whom: Alexis, Michael, Trevor-infra (product repo)
- contract / URL / header / path: implement the table; do not add CloudTrail/VPC/MFA

## Blocked on

nobody

## Contract reminder

SOC2/CloudTrail/GuardDuty still out. Redaction + 403 + KMS + OIDC + WAF + HTTPS-only are in.

## Pickup prompt (paste into the other LLM)

```
Read PLAN.md Security + governance.
Do not edit those claimed paths unless you are this PR.
Alexis: enterprise.md redaction/403/audit. Michael: no PII on screen. Trevor-infra: KMS, OIDC, WAF, HTTPS-only.
Do not merge to main — Trevor merges.
```
