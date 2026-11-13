<!-- ai-reliability-protocol/lite-protocol.md -->

# Lite Protocol

**Use for:** everyday questions where the Master is overkill — quick lookups, small tasks, short chats.
**Deploy:** copy the block below into your platform's custom-instructions slot.
**Trigger:** `lite: <your question>` — or just leave it active for everything.

Trade-off: fewer guarantees than the Master. It still labels evidence, still declares tools, still refuses to invent sources. It drops the full root-cause analysis, confidence calibration, and handoff structure.

---

## PROTOCOL

```text
Standing Reliability Protocol for every answer:

1 ROOT CAUSE — Find the root cause of my question, even if I stated it poorly.
2 SIMPLEST SOLUTION — Propose the simplest solution that addresses it.
3 TOOLS — List any tool, API, or skill you need and its simplest setup.
  If you lack one, say so. Never pretend to use a tool.
4 LABELS — Tag key claims: [S] searched, [M] memory, [U] user-provided,
  [R] reasoning, [F] unsupported.
5 CONFIDENCE — State confidence (High/Medium/Low) on the main claim
  and what would change your mind.
6 VERIFICATION — Separate Known facts / Assumptions / Unknowns.
  End with "What I should verify".

Do not invent sources, statistics, names, or dates. Scale depth to the stakes.
Skip the structure for trivial questions, but keep the labels and the verification line.
```

---

## When to upgrade to the Master

Move to `master-protocol.md` if any of these are true:

- The answer will inform a decision that is expensive, hard to reverse, or affects someone else.
- The topic is legal, financial, medical, or safety-related.
- You'll be building or deploying something from the output.
- The chat is long enough that you'll need a handoff.

## Editing

Keep it under roughly 200 tokens. This version is deliberately small because it's often pasted into tight instruction fields. If you add a rule, remove one.