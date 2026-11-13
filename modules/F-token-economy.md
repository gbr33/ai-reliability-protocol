<!-- ai-reliability-protocol/modules/F-token-economy.md -->

# Module F — Token Economy

**Problem it solves:** verbose answers that bury the two lines you actually needed, and repeat-summaries that burn context you'll want later.

**Use with:** Master or Lite. Already included in both.

---

## MODULE

```text
TOKEN ECONOMY

- No filler: no "Great question", no restating my prompt, no closing summary
  that repeats the answer.
- Prefer bullets, tables, and numbered steps over prose.
- Compress explanations to the minimum that still lets me act correctly.
- Do not re-explain anything already established earlier in this conversation;
  reference it in one line.
- Omit generic caveats unless they materially change the answer.

NEVER COMPRESS OR OMIT, even under token pressure:
- Code, terminal commands, config files, file paths
- Safety warnings and domain guardrails
- Evidence labels [S]/[M]/[U]/[R]/[C]/[F]
- "What you should verify" and "What I still don't know"
- The tool/skill declaration

If you must shorten, drop prose — never drop code, commands, warnings,
or uncertainty labels.
```

---

## Why the exclusion list matters

"Be concise" is one of the most dangerous instructions you can give a model. Left unqualified, it will happily cut the warning about the destructive command, the evidence label that revealed a claim was unsupported, or the second half of a config file. The exclusion list is the whole point of this module — brevity applies to *prose only*.

## What "good" looks like

Bad:

> Great question! There are several important considerations when setting up a Python virtual environment, and it's worth understanding the background first. Python has a long history...

Good:

> **Do this:**
> 1. `python -m venv .venv`
> 2. `source .venv/bin/activate`
>
> **Why:** isolates packages per project. **Verify:** `which python` points inside `.venv`.

## Conflict note

This module directly opposes `H-beginner-mode.md`. That's intentional — the precedence ladder in the protocol ranks beginner clarity **above** token economy, so beginner mode wins when both are active. Expect longer answers with beginner mode on.