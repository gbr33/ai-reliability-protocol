<!-- ai-reliability-protocol/modules/H-beginner-mode.md -->

# Module H — Beginner Mode

**Problem it solves:** instructions that assume knowledge you don't have — undefined jargon, "simply run the command," steps that skip what you should see on screen, and no path back when something goes wrong.

**Use with:** Master or Lite.
**Trigger:** append `— beginner mode`, or say "explain like I'm new."

---

## MODULE

```text
BEGINNER MODE

- Assume I have zero prior knowledge of this tool, language, or concept.
- Define every piece of jargon the first time it appears, in parentheses,
  in under 10 words.
- Number every action step. One action per step.
- Each step states: what to type/click → what I should see → what it means.
- Never say "simply", "just", or "obviously".
- Give exact strings to copy: file paths, commands, menu names, URLs.
- If a step can destroy data or cost money, flag it with ⚠ before the step.
- End with "If something went wrong:" listing the 2–3 most common failure points.
```

---

## Why "simply" is banned

"Simply" and "just" are the two words that most reliably signal a skipped step. They compress the exact part a beginner needs — the part the writer found obvious. Banning them forces the model to spell out the action or admit it's leaving something out.

## The three-part step

Every step must answer three questions, because a beginner can't infer any of them:

1. **What do I do?** — the exact string, never a description of it.
2. **What should I see?** — so you know it worked before moving on.
3. **What does it mean?** — so you're learning, not blindly following.

Example:

> **Step 3.** In the terminal, type `ls -la` and press Enter.
> **You should see:** a list of files, including hidden ones starting with a dot.
> **What it means:** `ls` lists directory contents; `-la` shows all files in long format.

## Conflict note

Beginner mode produces **longer** answers by design. The precedence ladder in the protocol ranks beginner clarity above token economy, so when both modules are active, beginner mode wins. Brevity applies to everything *except* the steps.

## Scope

Beginner mode is a per-conversation setting, not a permanent one. If it's on for a task where you already know the domain, turn it off — it costs tokens you'll want later.