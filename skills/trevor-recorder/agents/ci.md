# Mission: trevor-ci

**Id:** `trevor-ci`  
**Write:** `.github/` and `Makefile` only

You own CI that blocks a merge and deploys the SaaS from `main`. You do not merge. Trevor merges feature branches into `main`. Alexis/Michael do not write these YAML files.

Last year’s GHA (checked on `CanadaDevOpsCommunity2025`): HemoStat = Sphinx auto-commit to `main` (do not copy). Minions = SSH `:22` + compose redeploy of the demo (steal “CI ships the demo/UI”, not SSH). GenA11y = deploy on **every push** + EC2 + AKIA in `promote.yml` (steal terraform + curl URL; not AKIA/SSH/every-push). VRA and InnerAI had **no** workflows. We use OIDC, path-filtered tests, `deploy.yml` on `main` only.

Minions deployed the workload under test. We deploy **platform + demo-app tests + web** from `main`, not Grafana.

## Files to create

```
Makefile
.github/CODEOWNERS
.github/workflows/gitleaks.yml
.github/workflows/trivy.yml
.github/workflows/sdk.yml
.github/workflows/vault.yml
.github/workflows/web.yml
.github/workflows/infra.yml
.github/workflows/deploy.yml
```

## Makefile (Unix)

Targets: `test` (sdk pytest if `sdk/` exists), `vault` (pytest in `vault/` if exists), `web` (echo skip until `web/` exists), `demo` (`scripts/demo_pii_flight.sh` or skip), `plan` (`terraform plan` if `infra/` exists), `fmt`, `redact-check` (alias of `vault`).

Jobs skip cleanly if the directory is missing (greenfield). No PowerShell. No `make windows-*`.

## CODEOWNERS

Copy the block from [ownership.md](../ownership.md).

## Workflows

**gitleaks.yml** — every PR and `main`. Fail closed.

**trivy.yml** — `trivy fs .` every PR.

**sdk.yml** — `sdk/**` `demo-app/**` `contracts/**`: Python 3.12, `uv`, `pytest`, `bandit -r sdk`.

**vault.yml** — `vault/**` `contracts/**`: Python 3.12, `pytest`, `bandit -r vault`. Skip if `vault/` missing.

**web.yml** — `web/**`: Node 22, `pnpm lint`, Playwright vs fixtures. Skip if `web/` missing.

**infra.yml** — `infra/**`: terraform fmt, init (backend if configured, else `-backend=false`), validate. Plan if `AWS_ROLE_ARN` present.

**deploy.yml** — **push to `main` only**. OIDC. `terraform apply` `dev`, then `prod` with GitHub Environment protection. If `web/` exists: `pnpm build` + `aws s3 sync` + CloudFront invalidation. Never AKIA. Rollback = re-run this workflow on the previous successful commit (document in Makefile help).

Pin action SHAs if known; else major tags + TODO to SHA-pin.

Human checklist (README in Makefile help, do not automate unless asked): `main` protected, required checks, Trevor-only merge.

## Do not

CodePipeline. Per-PR AWS stacks. Skip gitleaks. Edit application code. Deploy from feature branches. SSH `:22`. AKIA. Hourly docs-commit CI. `make windows-*`.

## Done

- [ ] Empty tree: missing dirs skip, gitleaks/trivy still run
- [ ] `vault.yml` and `web.yml` exist (PLAN requires them)
- [ ] Lease released
