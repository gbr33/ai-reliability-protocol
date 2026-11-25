<!-- ai-reliability-protocol/CHANGELOG.md -->

# Changelog

All notable changes to the AI Reliability Protocol are recorded here.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
Versioning follows [Semantic Versioning](https://semver.org/):
**major** = changes that alter how the protocol behaves; **minor** = new modules or rules; **patch** = wording and clarity fixes.

Dates are recorded when a version is tagged. Replace `YYYY-MM-DD` with the actual release date at publish time.

---

## [Unreleased]

### Planned
- A one-page printable cheatsheet (PNG + PDF) for non-readers.
- A browser bookmarklet that assembles the protocol plus a typed question into one paste.
- A "diff reviewer" module for when the model is asked to review code it did not write.

---

## [1.3.0] — 2026-11-25

### Added
- USER OVERRIDES clause in LIMITS: the model may suspend a rule on request
  but must name it; the no-invent rule is never suspendable. Mirrored in the
  loader's fallback spine.
- TOOL AVAILABILITY clause in §3: models without retrieval must declare it
  once and must not use [S].

### Changed
- Intro line: "apply silently" narrowed to pure definitions and arithmetic.
- §6 SELF-RECHECK: now targets the least-confident claims instead of listing
  every claim, resolving the conflict with Module F. Sentinel string unchanged.
- Module K step 4: version disclosure required for the primary recommendation
  only; "version unverified" acceptable for secondary mentions.
- Loader rule 3: added redirect and truncation to the failure list.
- Loader rule 4: now checks version string as well as heading count.

### Unchanged
- Section heading count remains 14.

## [1.2.0] — 2026-11-13

### Added
- **Module F — Token Economy.** Filler removal with an explicit never-compress list covering code, commands, warnings, evidence labels, and verification lines.
- **Module G — Code & Terminal Output.** Complete-code rule, single copy-paste command blocks, explicit shell and working directory, "Expected result" and "If it fails" lines.
- **Module H — Beginner Mode.** Zero-knowledge framing, jargon definitions, three-part steps, banned words ("simply", "just", "obviously"), destructive-step warnings.
- **Module I — Handoff Document.** Nine-section self-contained resume block with a no-context test.
- **Precedence ladder** at the top of the protocol, resolving conflicts between brevity, completeness, and clarity.
- `system-prompt.xml` — tagged version of the full protocol for APIs, Custom GPTs, and Gems.

### Changed
- Master protocol restructured into numbered sections with headers, so individual rules can be located and edited.
- Added a `LIMITS` clause stating that platform safety policies override the protocol, and that the model must say when it has overridden.

### Fixed
- Long instruction blocks were previously competing with the user's actual question for attention; the precedence ladder and the "apply silently where it doesn't change the answer" clause reduce over-application on trivial questions.

---

## [1.1.0] — YYYY-MM-DD

### Added
- **Tool and skill declaration** section: the model must list every tool, API, dataset, or skill it needs, with the simplest configuration for each, and must explicitly state when it lacks one rather than pretending.
- **Evidence labels** `[S] [M] [U] [R] [C] [F]` applied to each key claim.
- **Confidence calibration**: level plus evidential basis plus "the weakest link in my reasoning is…" plus "what would change this answer is…".
- **Root-cause-first** step, including permission to ask up to three clarifying questions when the problem statement is genuinely ambiguous.
- **Domain guardrail** for legal, financial, medical, and safety-critical topics.

### Changed
- Renamed the "verification checklist" to end with "What you should verify" and "What I still don't know" for consistency across modules.

---

## [1.0.0] — YYYY-MM-DD

### Added
- Initial release.
- `master-protocol.md` — grounded answering, no invented sources, known facts / assumptions / unknowns separation.
- `lite-protocol.md` — short version for everyday questions and small instruction fields.
- `platform-setup.md` — paste instructions for ChatGPT, Claude, Gemini, API, Ollama, LM Studio, and generic apps.
- `README.md` with the 60-second setup, trigger-phrase table, and honest limits.

---

## How to contribute a change

1. Identify the specific hallucination or failure you hit — one concrete example.
2. Add or edit **one rule**, not a paragraph.
3. Check it against the precedence ladder; if it conflicts with an existing rule, update the ladder instead of adding a caveat.
4. Test the full protocol across at least three different models with the same five questions.
5. Note where models skip the new rule; tighten the wording rather than adding volume.
6. Record the change here under `[Unreleased]`, then move it to a version on release.

### Standing warning

Every addition costs attention for the rules already there. The protocol should stay under roughly 600 tokens. **If you add, cut.**
