# Invariants

These must stay in sync. Change them in ONE commit or verification breaks.

| Item | Value | Where it appears |
|---|---|---|
| Section heading count | 14 | ALL-IN-ONE.md, loader.md rule 4 |
| SELF-RECHECK sentinel | "Claims removed or downgraded in recheck" | ALL-IN-ONE.md §6 |
| MODULE K sentinel | "State your knowledge cutoff" | ALL-IN-ONE.md Module K |
| Protocol version | 1.3.0 | ALL-IN-ONE.md H1, CHANGELOG.md |

## If you add or remove a section
1. Update the count in ALL-IN-ONE.md (if stated there).
2. Update the count in loader.md rule 4.
3. Update the table above.
4. Add a CHANGELOG entry.
5. Run the smoke test (see SMOKE-TEST.md).
