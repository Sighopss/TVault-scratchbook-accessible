# Handoff — trevor-docs — split

- Date: 2026-08-21
- Human: Trevor
- Agent id: trevor-docs
- Branch: `trevor/docs/split`
- PR: 10
- Mission file: n/a (PLAN split + 2026 handbook)

## Claimed paths (collision)

```
PLAN.md
README.md
skills/FILL-CONSTITUTION.md
skills/trevor-recorder/agents/ci.md
skills/trevor-recorder/agents/demo.md
handoffs/trevor-docs-split.md
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

Anyone not editing the claimed paths.

## What I shipped

- files: equal 7-task split; PLAN **Handbook (2026)** maps P-01–P-15, threat model, system card, demo integrity, submission pack
- outputs / env **names**: none
- tests: none

## What I need

- from whom: Alexis, Michael, Trevor at product-repo time
- contract / URL / header / path: implement handbook rows already assigned to existing 1–7 tasks

## Blocked on

nobody

## Contract reminder

Still no Grafana, CloudTrail, VPC, MFA. Observability evidence = Flight Recorder + /health + 5xx.

## Pickup prompt (paste into the other LLM)

```
Read PLAN.md Three-person work and Handbook (2026).
Seven tasks each. Do not merge to main — Trevor merges.
```
