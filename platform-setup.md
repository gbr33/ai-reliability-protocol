<!-- ai-reliability-protocol/platform-setup.md -->

# Platform Setup

The protocol only works as a **persistent instruction**, not as a link. Paste it once into the slot below, then use the trigger phrase forever after.

> **Don't do this:** "Follow the instructions at github.com/…" — most chat apps can't fetch URLs, and the ones that can often retrieve the wrong file or invent its contents. Paste the text directly.

---

## Choosing a version

| Slot size | Use |
|---|---|
| Large (Custom GPT, Claude Project, Gemini Gem, API system prompt) | `master-protocol.md` |
| Small (a custom-instructions field with a tight character limit) | `lite-protocol.md`, or the Master with `<beginner_mode>` and `<handoff>` removed |
| No persistent slot available | Paste the Lite protocol at the top of each new chat |

Character limits on custom-instruction fields change between plans and releases. Check yours before assuming the Master fits; if it gets silently truncated, you'll lose whichever rules were at the end.

---

## ChatGPT

**Option A — Custom Instructions (applies to all chats)**
1. Settings → Personalization → Custom Instructions.
2. Paste the protocol into the field for how the model should respond.
3. Save. Use the trigger phrase in any chat.

**Option B — Custom GPT (recommended for the Master)**
1. Explore GPTs → Create.
2. Paste the protocol into **Instructions**. This field is much larger than Custom Instructions.
3. Name it something memorable, e.g. *Reliable*.
4. Use that GPT whenever you need the full protocol; keep normal chats for casual use.

Option B is better for the Master because the field is bigger and the protocol stays isolated from your general chat persona.

---

## Claude

1. Create a **Project** for the work it applies to.
2. Open Project → **Set project instructions**.
3. Paste the protocol. Save.
4. Every conversation inside that project inherits it.

For a lighter-weight approach, a **Style** can hold the tone-and-format parts (token economy, beginner mode), but it won't reliably carry the tool-declaration and evidence-labeling rules. Prefer Project instructions.

---

## Gemini

1. Open **Gems** → New Gem.
2. Paste the protocol into the instructions field.
3. Name it and save.
4. Start chats from that Gem.

---

## API (OpenAI, Anthropic, Gemini)

Send the protocol in the system role, before the conversation:

- **OpenAI:** the `system` message (Responses API: `instructions`).
- **Anthropic:** the top-level `system` parameter.
- **Gemini:** `system_instruction`.

Use `system-prompt.xml` for these — tagged instructions tend to survive long context better than plain prose.

Keep it to the first message only; repeating it every turn wastes tokens and can push the actual conversation out of the model's attention.

---

## Ollama

Create a model with the protocol baked in:

```bash
# write the Modelfile with the protocol on the SYSTEM line
cat > Modelfile <<'EOF'
FROM llama3.1
SYSTEM """
<paste the protocol text here>
"""
EOF

# build it under a new name
ollama create reliable -f Modelfile

# run it
ollama run reliable
```

---

## LM Studio

1. Open the **System Prompt** field in the right-hand panel.
2. Paste the protocol. Save it as a **preset** so you can switch between Reliable and normal modes.

---

## Any other chat app

Look for, in order of preference:

1. A system prompt or "instructions" field.
2. A project / workspace / folder-level instructions setting.
3. A custom persona, style, or agent configuration.
4. Nothing — in which case paste `lite-protocol.md` as the first message of each new chat, and accept that it may fade over a long conversation.

---

## Verifying it took effect

Test with a question that has a checkable factual answer and a plausible-but-wrong variant, such as a request for a specific statistic. A working deployment will:

- label the claim with an evidence tag,
- state a confidence level,
- and end with a "what you should verify" line.

If you get a plain confident answer with no labels, the protocol didn't load — check for truncation or a field that didn't save.

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| No labels appear | Protocol truncated, or pasted into a non-persistent field | Use Lite, or move to a Custom GPT / Project / Gem |
| Model ignores later rules | Protocol too long, diluted by context | Cut rules; keep under ~600 tokens |
| Model claims to use tools it lacks | Tool rule got truncated | Re-paste the `SOLUTION + TOOLS` section |
| Handoffs never happen | Model can't detect its own context limit | Ask explicitly at every milestone |
| Answers get terser over time | Conversation length diluting instructions | Restart with a handoff, or re-paste the protocol |