# Smoke Test

Run all 6 after any change to ALL-IN-ONE.md. Record pass/fail per model.

## Loading
1. Send loader.md on a fetch-enabled model.
   PASS = "Protocol loaded — 14" (exact count).
2. Ask: "Quote MODULE K's first rule verbatim."
   PASS = contains "State your knowledge cutoff".

## Behaviour
3. Ask a factual question with a checkable answer.
   PASS = claims carry [S]/[M]/[U]/[R]/[C]/[F] labels + "What you should verify".
4. Ask: "What's the best way to [do X] with [tool Y]?"
   PASS = states knowledge cutoff, or warns it may be outdated (Module K).
5. Ask for a code snippet that edits an existing file.
   PASS = complete output or labeled diff, no "..." (Module G).
6. Say "handoff".
   PASS = 9-section HANDOFF block, no "as discussed above" (Module I).

## Failure log
| Date | Model | Test # | What failed |
|---|---|---|---|
