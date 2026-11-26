# INVARIANTS.md

These values must stay in sync across files. A mismatch here is not a style
problem — it is the integrity check failing, and every downstream verification
becomes unreliable.

**Change them in ONE commit, or the system reports false failures.**

---

## The contract

| Item | Value | Where it appears | Verified by |
|---|---|---|---|
| Protocol version | **v1.4.0** | `ALL-IN-ONE.md` H1 · `loader.md` rule 4 · `CHANGELOG.md` | Loader rule 4 |
| Section heading count | **15** | `loader.md` rule 4 | Loader rule 4 · `grep -c '^## '` |
| SELF-RECHECK sentinel | `Claims removed or downgraded in recheck` | `ALL-IN-ONE.md` §6 | Smoke test 3 |
| MODULE K sentinel | `State your knowledge cutoff` | `ALL-IN-ONE.md` Module K | Smoke test 2 |
| FINAL GATE sentinel | `Gate: pass —` | `ALL-IN-ONE.md` §7 | Smoke test 4 |
| Override phrase | `Suspending protocol rule: [rule name]` | `ALL-IN-ONE.md` LIMITS · `loader.md` spine | Smoke test 10 |
| Loader failure phrase | `Protocol not loaded — using fallback spine.` | `loader.md` rule 3 | Smoke test 12 |
| Version-mismatch phrase | `Protocol version mismatch — reload required.` | `loader.md` rule 4 | Smoke test 13 |
| Fetch URL | `https://raw.githubusercontent.com/gbr33/ai-reliability-protocol/refs/heads/main/ALL-IN-ONE.md` | `loader.md` rule 1 | Live fetch |

**Sentinel** means: exact string, character for character. Paraphrasing a
sentinel breaks the check that depends on it, silently.

---

## The 15 sections

This is the canonical list. If the count is 15, these are the headings that
make it 15. If your file's headings differ from this list, one of the two is
wrong — reconcile before relying on the count.

| # | Heading |
|---|---|
| 1 | `## PRECEDENCE` |
| 2 | `## 1 ROOT CAUSE` |
| 3 | `## 2 SIMPLEST SOLUTION + TOOLS` |
| 4 | `## 3 GROUNDED ANSWER + LABELS` |
| 5 | `## 4 CONFIDENCE` |
| 6 | `## 5 VERIFICATION` |
| 7 | `## 6 SELF-RECHECK` |
| 8 | `## 7 FINAL GATE` |
| 9 | `## MODULE F — TOKEN ECONOMY` |
| 10 | `## MODULE G — CODE & TERMINAL` |
| 11 | `## MODULE H — BEGINNER MODE` |
| 12 | `## MODULE I — HANDOFF` |
| 13 | `## MODULE K — TEMPORAL GROUNDING & FRESHNESS` |
| 14 | `## DOMAIN GUARDRAIL` |
| 15 | `## LIMITS` |

**Note:** there is no Module J by design. SELF-RECHECK was promoted from a
module to core section 6.

**Counting rule:** the check counts `##` headings only. `#` (the H1) and
`###` (sub-headings) are excluded. Do not add a `###` line and expect the
count to change — it won't, correctly.

---

## Version numbering

Semantic versioning, with a narrow definition of what counts as breaking.

| Change | Bump | Count changes? |
|---|---|---|
| Wording, clarity, typo fix | patch (v1.4.1) | No |
| New rule *inside* an existing section | patch (v1.4.1) | No |
| Rule removed or merged into another section | patch (v1.4.1) | No |
| New `##` section added | minor (v1.5.0) | **Yes** |
| `##` section removed | minor (v1.5.0) | **Yes** |
| Sentinel string reworded | **major** (v2.0.0) | No |
| Precedence ladder reordered | **major** (v2.0.0) | No |
| Protocol fundamentally restructured | **major** (v2.0.0) | Likely |

**Why sentinel changes are major:** they break the verification tests that
depend on them. A patch that silently invalidates smoke tests 2–4 is not a
patch.

---

## Procedure: adding or removing a `##` section

1. Edit `ALL-IN-ONE.md`.
2. Bump the H1 version (minor, per the table above).
3. Update the count in `loader.md` rule 4 **and** the version.
4. Update the "Section heading count" and "Protocol version" rows above.
5. Update the 15-section list above.
6. Add a `CHANGELOG.md` entry.
7. Commit all four files together.
8. Run `SMOKE-TEST.md` tests 1–4.
9. Run the live check below.

---

## Procedure: changing a sentinel string

Do not. If you must:

1. Treat it as a **major** version bump.
2. Update the sentinel in `ALL-IN-ONE.md`.
3. Update the sentinel row above.
4. Update the corresponding smoke test in `SMOKE-TEST.md`.
5. Update the fallback spine in `loader.md` if the sentinel appears there.
6. Commit everything together and re-run the full smoke test.

A sentinel changed in the protocol but not in the smoke test means the test
now asserts something the protocol no longer says — a false failure on a
healthy system, which is worse than no test.

---

## Live verification commands

Run after every push that touches `ALL-IN-ONE.md`.

```bash
# 1. Version string — must print exactly: # Reliability Protocol v1.4.0
curl -s https://raw.githubusercontent.com/gbr33/ai-reliability-protocol/refs/heads/main/ALL-IN-ONE.md | head -1

# 2. Section count — must print exactly: 15
curl -s https://raw.githubusercontent.com/gbr33/ai-reliability-protocol/refs/heads/main/ALL-IN-ONE.md | grep -c '^## '

# 3. Sentinels present — must print 3
curl -s https://raw.githubusercontent.com/gbr33/ai-reliability-protocol/refs/heads/main/ALL-IN-ONE.md \
  | grep -cE 'Claims removed or downgraded in recheck|State your knowledge cutoff|Gate: pass —'
```

If any of the three returns a different value, **do not update `loader.md` to
match the broken file.** Fix the file first. The loader asserts what the file
*should* be, not what it currently is.

---

## Conflict resolution

If `ALL-IN-ONE.md` and `loader.md` disagree:

- **The file wins on content.** It is the source of truth for the rules.
- **The loader wins on expected values.** It is the source of truth for what
  a correct load looks like.
- If they cannot both be right, the correct move is to revert to the last
  commit where they agreed, then reapply the change properly.

Never "fix" a mismatch by editing only one side.

---

## Historical note

| Version | Sections | Note |
|---|---|---|
| v1.2.0 | 14 | Modules F–I + precedence ladder |
| v1.3.0 | 14 | Module K added; not tagged |
| v1.3.1 | — | Never released; changes folded into 1.4.0 |
| **v1.4.0** | **15** | §7 FINAL GATE · version string in H1 · override clause |

---

## What this file cannot do

This file documents invariants. It does not enforce them. Enforcement comes
from the loader's rule 4 (version + count) and `SMOKE-TEST.md`. If neither is
run, this file is accurate and useless.
