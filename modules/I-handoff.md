<!-- ai-reliability-protocol/modules/I-handoff.md -->

# Module I — Handoff Document

**Problem it solves:** losing hours of context when a chat hits its limit, and starting the next one with "so, where were we?" — while the model dutifully re-explains things you already decided.

**Use with:** Master or Lite.
**Trigger:** type `handoff`, or ask at a milestone.

---

## MODULE

```text
HANDOFF DOCUMENT

Trigger this when: I say "handoff", OR we finish a major milestone,
OR I ask for one.

Produce a single self-contained block titled HANDOFF, with these sections:

1. OBJECTIVE — the original goal, one paragraph.
2. CURRENT STATE — what is done, what works, what is broken.
3. DECISIONS MADE — key choices and why, so the next chat doesn't relitigate them.
4. ARTIFACTS — every file, path, config, command, or code block produced,
   complete and copy-pasteable.
5. FAILED ATTEMPTS — what we tried that did not work, and why.
6. OPEN QUESTIONS — unresolved unknowns.
7. NEXT STEPS — the next 3–5 concrete actions, in order.
8. CONSTRAINTS — tools available, limits, preferences, domain guardrails in force.
9. RESUME PROMPT — one short paragraph I can paste into a new chat to continue.

Rules for the handoff:
- It must make sense with NO access to this conversation.
  No "as discussed above", no "the file we made".
- Include the full text of any code or file, not a description of it.
- Keep it under ~800 words unless artifacts require more.
- Do not include your own commentary or apologies.
```

---

## The self-containment test

Before you close a chat, apply this test: **paste the handoff into a fresh chat with no other context. Does it work?**

If the new model asks "which file?" or "what framework?" — the handoff failed. The usual culprits are phrases like "the config we set up" instead of the config itself, and "continue from where we left off" instead of concrete next steps.

## Why "Failed Attempts" earns its place

It's the section people skip and the one that saves the most time. Without it, the next chat cheerfully proposes the approach that already burned two hours — and you have to rediscover why it doesn't work.

## Why "Decisions Made" matters

Long chats accumulate decisions: library chosen, schema fixed, naming convention agreed. If those aren't recorded with their reasons, the next chat will propose alternatives and you'll relitigate settled ground.

## Honest limitation

**The model cannot reliably detect its own context limit.** It does not know how much room is left, so it will rarely volunteer a handoff at the right moment. Don't rely on that. Request a handoff **at every milestone** — it's cheap, and a slightly stale handoff is far better than a truncated one.

## Cadence suggestion

- After each working milestone → quick handoff.
- Before switching topics → handoff.
- Before any long break → handoff.
- When you notice the model repeating itself or forgetting earlier context → handoff *immediately*, before it degrades further.