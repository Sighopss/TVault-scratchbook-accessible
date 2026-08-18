# Mission: trevor-ci

**Id:** `trevor-ci`  
**Write:** `.github/` and `Makefile` only

You own the merge gate so parallel agents cannot land secrets or broken schema emits.

## Files to create

```
Makefile
.github/CODEOWNERS
.github/workflows/gitleaks.yml
.github/workflows/trivy.yml
.github/workflows/sdk.yml
.github/workflows/infra.yml
.github/workflows/deploy.yml
```

## Makefile (Unix)

Targets: `test` (sdk pytest), `demo` (calls `scripts/demo_pii_flight.sh` if present else echo skip), `plan` (`cd infra && terraform plan` if infra exists), `fmt`.

No PowerShell. No `make windows-*`.

## CODEOWNERS

Copy the block from [ownership.md](../ownership.md).

## Workflows

**gitleaks.yml** — every PR and `main`. Fail closed.

**trivy.yml** — `trivy fs .` every PR.

**sdk.yml** — on `sdk/**` and `contracts/**`: Python 3.12, `uv sync`, `pytest`. `bandit -r sdk/src`.

**infra.yml** — on `infra/**`: terraform fmt, init -backend=false, validate. Plan only if `AWS_ROLE_ARN` secret exists (OIDC).

**deploy.yml** — **push to `main` only**. OIDC to AWS. `terraform apply` for `dev` then optional `prod` with environment protection. Never deploy from feature branches. Never store AKIA.

Pin action SHAs if you know them; otherwise pin major tags and leave a TODO to SHA-pin.

## Do not

Add CodePipeline. Add per-PR AWS stacks. Skip gitleaks. Edit application code.

## Done

- [ ] Empty workflows still run on a greenfield tree (jobs skip gracefully if dir missing)
- [ ] README section in Makefile help
- [ ] Lease released
