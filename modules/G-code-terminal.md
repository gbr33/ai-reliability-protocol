<!-- ai-reliability-protocol/modules/G-code-terminal.md -->

# Module G — Code & Terminal Output

**Problem it solves:** truncated code with `...` in the middle, command blocks interleaved with explanation so nothing can be copied, missing shell context, and fragments that silently break the file you're editing.

**Use with:** Master or Lite. Already included in both.

---

## MODULE

```text
CODE AND TERMINAL OUTPUT

- Output code COMPLETE. Never use "...", "rest of the code here",
  or "unchanged parts omitted".
- One fenced block per file, with the file path on the first line as a comment.
- Terminal commands: give ONE single copy-paste block, in execution order,
  each line prefixed with a comment saying what it does.
- State the shell explicitly (bash / zsh / PowerShell / cmd)
  and the working directory.
- Include prerequisite install and setup commands in the same block, in order.
- Do not interleave explanations inside the command block;
  explain after the block, briefly.
- If editing an existing file, output the FULL updated file, not a fragment —
  or clearly label a diff with exact context lines.
- After the block, add one line: "Expected result:" describing what success looks like.
- Add a short "If it fails:" line with the most likely error and the fix.
```

---

## The two failure modes this fixes

**1. Truncation.** A model that writes `# ... rest of your code here ...` has produced something that cannot be run and cannot be safely pasted. The rule is absolute: complete output or an explicit labeled diff, never an ellipsis.

**2. Interleaving.** When explanation sits between commands, nothing can be selected and pasted. The block must be contiguous and executable as-is, with commentary outside it.

## What "good" looks like

**Set up the project (bash, run from `~/projects/myapp`):**

```bash
# create and enter the virtual environment
python -m venv .venv
source .venv/bin/activate

# install pinned dependencies
pip install -r requirements.txt

# start the dev server on port 8000
python manage.py runserver 8000
```

**Expected result:** server prints `Starting development server at http://127.0.0.1:8000/`.
**If it fails:** `command not found: python` means Python isn't on PATH — try `python3` and `pip3`.

## Diff mode

When a file is long and only a few lines change, full output wastes context. Then:

```diff
--- a/config/settings.py
+++ b/config/settings.py
@@ -12,6 +12,7 @@
 DATABASES = {
     "default": {
         "ENGINE": "django.db.backends.postgresql",
+        "NAME": "myapp",
         "HOST": "localhost",
     }
 }
```

State the file path, keep three lines of context, and never elide the changed lines themselves.