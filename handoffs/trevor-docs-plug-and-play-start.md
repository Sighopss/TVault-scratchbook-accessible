# Handoff — trevor-docs — plug-and-play-start

- Date: 2026-08-19
- Human: Trevor
- Agent id: trevor-docs
- Branch: `trevor/docs/plug-and-play-start`
- PR: 7
- Mission file: n/a (scratchpad format, not a recorder mission)

## Claimed paths (collision)

```
START.md
AGENTS.md
README.md
PLAN.md
skills/FILL-CONSTITUTION.md
skills/INDEX.md
skills/lane-constitution/
handoffs/
.github/pull_request_template.md
.gitignore
skills/trevor-recorder/handoffs.md
skills/trevor-recorder/parallel.md
skills/trevor-recorder/workflow.md
skills/trevor-recorder/SKILL.md
skills/trevor-recorder/agents/ci.md
```

## Do not touch

```
vault/
web/
sdk/
demo-app/
infra/
scripts/
```

Do not author `skills/<alexis-lane>/` or `skills/<michael-lane>/` bodies.

## Safe to run in parallel with

Alexis/Michael filling their own constitutions (their `skills/<lane>/` only). Any agent that does not edit the files in Claimed paths.

## What I shipped

- files: `START.md`, `skills/FILL-CONSTITUTION.md`, `handoffs/README.md`, `handoffs/PR.example.md`, PR template, `progress.md` in lane-constitution
- outputs / env **names**: none
- tests: none (docs/format)

## What I need

- from whom: Alexis, Michael
- contract / URL / header / path: they fill their own constitutions via FILL-CONSTITUTION.md; they write a `handoffs/<name>-<id>-<slug>.md` on every later PR

## Blocked on

nobody

## Contract reminder

Scratchpad is not the product repo. No `sdk/` `vault/` `web/` here.
