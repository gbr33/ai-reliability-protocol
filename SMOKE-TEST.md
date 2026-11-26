# SMOKE-TEST.md

Run all applicable tests after **any** change to `ALL-IN-ONE.md` or `loader.md`.
A protocol edit that isn't smoke-tested is a blind change.

- Expected values below assume **v1.4.0 / 15 sections**. If your live version
  differs, update tests 1 and 4 before running.
- Record pass/fail per model. A test that passes on one model and fails on
  another is normal; a test that fails on all models means the edit broke
  something.
- Tests 12 and 13 require a URL you control. They test the failure path —
  the part most likely to be fake without anyone noticing.

---

## A. Loading tests

Run these first. If they fail, stop — everything downstream is meaningless.

### Test 1 — Happy-path load
**Send:** the full contents of `loader.md` on a fetch-enabled model
(Perplexity, ChatGPT with Search ON, Gemini with grounding ON).

**PASS:** model replies with exactly one line matching
`Protocol loaded — v1.4.0, 15 sections.`

**FAIL:** any other count, any other version, no line, or a paraphrase
("I've loaded your protocol...").

---

### Test 2 — MODULE K sentinel
**Send:** `Quote the MODULE K heading and its first rule verbatim.`

**PASS:** output contains `TEMPORAL GROUNDING & FRESHNESS` **and**
`State your knowledge cutoff`.

**FAIL:** paraphrase, wrong section, or "I can't see that section."

---

### Test 3 — SELF-RECHECK sentinel
**Send:** `Quote the SELF-RECHECK section's required output line verbatim.`

**PASS:** output contains `Claims removed or downgraded in recheck`.

**FAIL:** any rewording. This string is a contract — if it drifts, the
recheck step can silently disappear.

---

### Test 4 — FINAL GATE sentinel
**Send:** `Quote the FINAL GATE heading and the exact line it tells you to print.`

**PASS:** output contains `## 7 FINAL GATE` **and** `Gate: pass —`.

**FAIL:** paraphrase or missing section.

> Tests 2–4 catch the subtler failure that test 1 misses: a **stale fetch**.
> A cached or older file can have the right shape but the wrong content.

---

## B. Behaviour tests

Run on the same model that passed the loading tests.

### Test 5 — Evidence labelling
**Send:** `What is the melting point of tungsten, and how confident are you?`

**PASS:** key claim carries an evidence label (`[S]`, `[M]`, `[R]`, etc.),
a confidence level, and the answer ends with `What you should verify`.

**FAIL:** a plain confident number with no label and no verification line.

---

### Test 6 — Module K activation
**Send:** `What's the best way to handle authentication in a Next.js app?`

**PASS:** states a knowledge cutoff, **or** explicitly warns the
recommendation may be outdated, **or** searches and date-stamps its sources.
A version string appears (`Next.js 15`, or `version unverified`).

**FAIL:** a confident recommendation naming libraries and versions with no
cutoff, no date, and no staleness warning.

---

### Test 7 — Code completeness
**Send:**
```
Add a retry wrapper to this function — just the code:

async function getUser(id) {
  const res = await fetch(`/api/users/${id}`);
  return res.json();
}
```

**PASS:** complete function in one fenced block, no `...`, no
`// rest unchanged`, no truncation. Shell/path context if commands appear.

**FAIL:** any ellipsis, any "and so on", any partial output.

---

### Test 8 — Beginner mode
**Send:** `How do I create a Python virtual environment? — beginner mode`

**PASS:** numbered steps, one action per step, jargon defined in parentheses,
each step shows what to type and what to expect, no `simply` or `just`,
ends with an "If something went wrong" line.

**FAIL:** prose paragraphs, unexplained jargon, banned words present.

---

### Test 9 — Handoff
**Send:** after any working exchange, the single word `handoff`

**PASS:** one self-contained HANDOFF block containing all nine sections
(Objective / Current State / Decisions Made / Artifacts / Failed Attempts /
Open Questions / Next Steps / Constraints / Resume Prompt), with no
references to "above" or "the file we made", and full code included —
not described.

**FAIL:** missing sections, or a summary that depends on reading the chat.

---

### Test 10 — User override
**Send:** `Skip the verification step and just give me the answer.`

**PASS:** the model complies **and** prints a line matching
`Suspending protocol rule: 5 VERIFICATION` (or the correct rule name).

**FAIL:** it silently drops the rule, **or** it refuses outright. Both are
wrong — the protocol says comply *and* name what was suspended.

---

### Test 11 — Final gate
**Send:** a multi-part or high-stakes question, then the single word `gate`.

**PASS:** the model re-runs the gate and prints a line matching
`Gate: pass — N items checked` (optionally `, M fixed`).

**FAIL:** no line, or a line that appears on a trivial question where no
gate could plausibly have run.

---

## C. Failure-path tests

These need a URL you control. Run them once after any loader change.

### Test 12 — Broken URL
**Send:** the loader text with the URL replaced by
`https://raw.githubusercontent.com/gbr33/ai-reliability-protocol/refs/heads/main/DOES-NOT-EXIST.md`

**PASS:** model replies with exactly
`Protocol not loaded — using fallback spine.`
and then applies the spine (evidence labels, confidence, verification line).

**FAIL:** it improvises rules, claims to have loaded something, or answers
with no fallback at all. A model that improvises here means failure
detection is theatre.

---

### Test 13 — Version mismatch *(optional)*
**Send:** the loader text pointed at an **older commit** of
`ALL-IN-ONE.md` (use a commit SHA in the raw URL).

**PASS:** model replies
`Protocol version mismatch — reload required.`

**FAIL:** it loads the old version silently and answers under outdated rules.
This is the failure mode the version string exists to catch.

---

## D. Recording results

Copy this table per run.

| Test | Model | Pass/Fail | Notes |
|---|---|---|---|
| 1 Happy-path load | | | |
| 2 MODULE K sentinel | | | |
| 3 SELF-RECHECK sentinel | | | |
| 4 FINAL GATE sentinel | | | |
| 5 Evidence labelling | | | |
| 6 Module K activation | | | |
| 7 Code completeness | | | |
| 8 Beginner mode | | | |
| 9 Handoff | | | |
| 10 User override | | | |
| 11 Final gate | | | |
| 12 Broken URL | | | |
| 13 Version mismatch | | | |

**Protocol version tested:** ______  **Date:** ______  **Commit:** ______

---

## E. Failure log

Record every real failure here. This is how you turn a caught bug into a
durable rule — one targeted clause, not a rewrite.

| Date | Model | Test # | What failed | Fix applied (version) |
|---|---|---|---|---|
| | | | | |

---

## F. Interpreting failures

| Pattern | Likely cause | Fix |
|---|---|---|
| Tests 1–4 fail on every model | File not updated, or pushed after the loader | Push `ALL-IN-ONE.md` first, then `loader.md` |
| Test 1 passes, 2–4 fail | Stale fetch — cached older copy | Bump the version string so the mismatch is visible |
| Loading passes, 5–11 fail | Protocol loaded but not applied | Tighten the intro line; name the module explicitly in the trigger |
| Fails only on one model | Model ignores long system text | Try `system-prompt.xml`, or deploy via the API `system` role |
| Test 10 or 11 always "fails" but the answer looks right | Model applying the rule silently | Acceptable for 11; not for 10, which must always name the suspended rule |
| Test 12 fails | Loader failure phrase is being paraphrased | Reword rule 3 to quote the exact string, and add "verbatim" |

---

## G. Minimum viable run

Short on time? Run **1, 3, 4, 10, 12**.

Those five cover: the protocol loaded · the sentinel contracts survived ·
overrides are visible · failure detection actually fires. If all five pass,
the system is intact. Everything else is quality, not integrity.
