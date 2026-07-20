---
name: prompt-shortcut
description: Personal prompt launcher. Use when the user runs /prompt-shortcut (or asks for "my prompts", "favorite prompts", or a prompt menu) to list their saved prompts as a clickable menu and run the chosen one in the current session. Also used by the shortcut commands /continue, /summarize, /package, and by /prompt-shortcut <name> to run a specific prompt directly without the menu.
---

# Prompt Shortcut

A personal launcher for the user's favorite prompts. Each prompt is one file in
the `prompts/` folder next to this SKILL.md. This skill lists those files, lets
the user pick one from a clickable menu, and runs it in the current session.

Adding a new prompt is just dropping a file in `prompts/` — no edit to this file
is needed. That scalability is the whole point of this skill, so never hardcode
a prompt list here.

## Where prompts live

All prompts are markdown files in `${CLAUDE_SKILL_DIR}/prompts/`. `${CLAUDE_SKILL_DIR}`
expands to this skill's own directory wherever it is installed (global
`~/.claude/skills/`, a project `.claude/skills/`, or a plugin cache), so the
prompts folder is always found.

Each prompt file's YAML frontmatter has:

- `title` — short label shown in the menu
- `description` — one line shown under the title
- `shortcut` — (optional) the direct slash-command name, e.g. `continue`

The file's body (below the frontmatter) is the actual prompt text — that is what
gets executed. Ignore any file whose name starts with `_` (e.g. `_TEMPLATE.md`).

## How to run

### Step 1 — did the user name a specific prompt?

A specific prompt is requested when:

- `$ARGUMENTS` is non-empty (e.g. `/prompt-shortcut continue`), or
- a shortcut command invoked this skill and named a prompt (e.g. the `/continue`
  command asks for the `continue` prompt).

If so: match the requested name to a prompt file by **filename without `.md`**
*or* by its **`shortcut`** field. If you find a match, go straight to
**Step 4** with that file — do **not** show the menu. If the name matches
nothing, briefly list what is available (titles) and stop.

### Step 2 — enumerate the prompts

Use **Glob** with path `${CLAUDE_SKILL_DIR}` and pattern `prompts/*.md`. Then
**Read** each file whose name does not start with `_`, and pull its `title` and
`description` from the frontmatter. Sort them by filename for a stable order.

### Step 3 — present the menu (adapt to the count)

AskUserQuestion shows at most four clickable options, so adapt:

- **4 or fewer prompts** → call **AskUserQuestion** once, single-select, with one
  option per prompt: `label` = the `title`, `description` = the prompt's
  `description`. (AskUserQuestion automatically adds an "Other" choice, so the
  user can also type a custom request or a prompt name that is not shown.)

- **More than 4 prompts** → buttons can't hold them all, so print a numbered list
  instead and let the user reply with a number or name:

  ```
  Pick a prompt — reply with a number or name:
    1. <title> — <description>
    2. <title> — <description>
    ...
  ```

  Then wait for the reply and map it back to the file.

### Step 4 — execute the chosen prompt

Read the selected file's **body** — everything below the closing `---` of the
frontmatter — and treat it as the user's request for this turn. Carry it out
directly in this session.

Do not summarize or restate the prompt before acting, and do not ask "should I
proceed?" unless the prompt body itself calls for confirmation. The body is
authoritative; run it as written.

## Notes

- This skill carries its `prompts/` folder with it, so it behaves identically
  across global, project, and plugin/marketplace installs.
- Shortcut commands in `commands/` only tell this skill which prompt to run; the
  prompt text itself lives only in `prompts/` (single source of truth).
- If `$ARGUMENTS` or a shortcut names a prompt that does not exist, surface the
  available titles instead of guessing.
