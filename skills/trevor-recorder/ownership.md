# Ownership fences

Trevor may **write**: `sdk/`, `demo-app/`, `infra/`, `scripts/`, `.github/`, `Makefile`

Trevor may **read**, and must **not write** except hour-0 with Alexis and Michael: `contracts/`

Trevor must **never write**: `vault/` (Alexis), `web/` (Michael), `PRODUCT.md`, `DESIGN.md`

If the task needs `vault/` or `web/`, write a handoff note (see [handoffs.md](handoffs.md)) and stop. Do not patch the other lane.

## CODEOWNERS (target)

```
/contracts/  @trevor @alexis @michael
/sdk/        @trevor
/demo-app/   @trevor
/infra/      @trevor
/.github/    @trevor
/vault/      @alexis
/web/        @michael
```

Schema PRs need all three people. Trevor-only PRs need Trevor + one other.
