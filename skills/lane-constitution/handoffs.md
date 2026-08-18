# Handoffs

You do not implement another lane. You leave them a file they can drop into their agent.

Write `handoffs/FROM-<your-name>-<id>.md`. Template:

```markdown
# Handoff from <your-name>-<id>
Date:
Blocked on: <other-human> | Human

## What I shipped
- paths:
- outputs / env vars:

## What I need
- from whom:
- contract / URL / header / path:

## Contract reminder
<only the interface you own — not another lane’s internals>
```

## Direction table

Fill rows for **your** I/O with the other two humans. After hour 0, HTTP is `contracts/http.md` — do not invent routes. UI labels = schema names from `contracts/` after hour 0.
