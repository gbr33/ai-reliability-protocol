<!-- ai-reliability-protocol/README.md -->

## The two files that matter
- **ALL-IN-ONE.md** — the protocol (the rules).
- **loader.md** — the prompt that loads it (the transport).

Everything else in this repo is reference, modules, or history.

# AI Reliability Protocol

A copy-paste prompt protocol that makes AI models **show their work**: root-cause the problem, declare the tools they need, label the evidence behind each claim, and hand off cleanly when the chat runs out.

No install. No API key. No code.

---

## What this does

- Forces **root-cause diagnosis** before answering, so the model solves your real problem.
- Makes the model **declare every tool or skill it needs** — and say so honestly when it lacks one, instead of pretending.
- **Labels every key claim** by evidence source, so guesses stop looking identical to facts.
- Requires **confidence statements** and "what would change this answer."
- Compresses **filler**, never compresses code, commands, warnings, or uncertainty.
- Produces a **handoff document** so a new chat can continue without the old context.

## What this does not do

This does **not** eliminate hallucinations. Nothing does. It makes them **easier to catch**: the model has to expose its assumptions, name its tools, and end with a list of things you should verify. Treat every output as a draft that needs checking, especially for health, legal, financial, and safety topics.

---

## 60-second setup

1. Pick a protocol file:
   - `master-protocol.md` — complex problems, decisions, research, high stakes.
   - `lite-protocol.md` — everyday questions, short and fast.
2. Copy the text inside the `PROTOCOL` block (not the whole file).
3. Paste it into your platform's **custom instructions / system prompt** slot. See `platform-setup.md` for exact steps per platform.
4. Ask anything, prefixed with the trigger phrase:

```text
Apply the Reliability Protocol to: <your question>
```

That's it. The protocol is now a standing instruction — you don't paste it again.

> **Do not** just send the model a GitHub URL and say "follow these." Most chat apps can't fetch it, and the ones that can often mangle it. Paste it once into the instruction slot instead.

---

## Trigger phrases

| You type | What happens |
|---|---|
| `Apply the Reliability Protocol to: …` | Full protocol |
| `lite: …` | Short protocol only |
| `… — beginner mode` | Adds beginner explanations (Module H) |
| `… — just the code` | Complete code, minimal prose (Modules F + G) |
| `handoff` | Emits a resumable HANDOFF block (Module I) |

---

## File map

| File | Purpose |
|---|---|
| `master-protocol.md` | Full protocol, consolidated. Start here. |
| `lite-protocol.md` | Short protocol for quick questions. |
| `system-prompt.xml` | Same protocol in XML tags, for APIs and Custom GPTs. |
| `modules/F-token-economy.md` | Filler reduction without losing substance. |
| `modules/G-code-terminal.md` | Complete code, single copy-paste command blocks. |
| `modules/H-beginner-mode.md` | Zero-knowledge step-by-step explanations. |
| `modules/I-handoff.md` | Resume document for the next chat. |
| `platform-setup.md` | Per-platform paste instructions. |
| `CHANGELOG.md` | Version history. |

**Note on length:** custom-instruction fields are usually length-limited. If the Master doesn't fit, use the Lite version, or deploy the Master as a Custom GPT / Claude Project / Gemini Gem, where you have more room.

---

## The precedence rule

The protocol's rules can conflict with each other. "Use fewer tokens" fights with "paste all the code" and "explain like I'm a beginner." So the protocol states an explicit order:

```text
safety & accuracy  >  code/command completeness  >  handoff completeness
                   >  beginner clarity  >  token economy
```

Never trade away the first three to save tokens. This single line is what keeps the modules from cancelling each other out.

---

## Honest limits

- **The model can't reliably detect its own context limit.** Don't wait for it to volunteer a handoff. Ask for one at every milestone — it's cheap and a stale handoff beats a truncated one.
- **Beginner mode and token economy pull in opposite directions.** Beginner explanations need words. Expect longer answers when beginner mode is on; brevity applies to everything except the steps.
- **Safety policies override the protocol.** The protocol tells the model to say so when that happens, rather than silently ignoring instructions.
- **A confident answer is not a verified answer.** The evidence labels tell you which parts to check. Check them.

---

## Contributing

Keep it short. The protocol should stay under roughly 600 tokens to survive dilution. When you find a recurring hallucination, add **one targeted rule** rather than rewriting. Record the change in `CHANGELOG.md`.

## License

This repository is for personal use only.
No license is granted. All rights reserved.
