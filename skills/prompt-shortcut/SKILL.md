---
name: prompt-shortcut
description: Personal prompt launcher. Use when the user runs /prompt-shortcut (or asks for "my prompts", "favorite prompts", or a prompt menu) to list their saved prompts as a clickable menu and run the chosen one in the current session. Also used by the shortcut commands /continue, /conclusion, /packaging, and /add-prompt (which interactively saves a new prompt into the menu), and by /prompt-shortcut <name> to run a specific prompt directly without the menu.
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

**Special case — adding a prompt:** if this was invoked via `/add-prompt`, or `$ARGUMENTS` is `add`, or the user asked to *save/add* a new favorite prompt, do not show the menu — go to the **Adding a new prompt** section instead.

### Step 2 — enumerate the prompts

Use **Glob** with path `${CLAUDE_SKILL_DIR}` and pattern `prompts/*.md`. Then
**Read** each file whose name does not start with `_`, and pull its `title` and
`description` from the frontmatter. Sort them by filename for a stable order.

### Step 3 — present the menu (adapt to the count)

The menu always includes one permanent entry on top of the prompts:
**"➕ Add a new prompt"** — picking it launches the wizard in the *Adding a new
prompt* section. So the total number of options is (number of prompts) + 1.

AskUserQuestion shows at most four clickable options, so adapt:

- **Total ≤ 4 (3 prompts or fewer, plus Add)** → call **AskUserQuestion** once,
  single-select: one option per prompt (`label` = the `title`, `description` =
  the prompt's `description`), then a final option `label` = "➕ Add a new prompt",
  `description` = "Save a new favorite prompt into the menu."

- **Total > 4 (4 prompts or more)** → buttons can't hold them all, so print a
  numbered list and let the user reply with a number, a name, or `add`:

  ```
  Pick a prompt — reply with a number, name, or "add":
    1. <title> — <description>
    2. <title> — <description>
    ...
    add  ➕ Add a new prompt
  ```

If the user picks **Add a new prompt** (or replies `add`), go to the **Adding a
new prompt** section — do not try to run a prompt body.

### Step 4 — execute the chosen prompt

(Only for a real prompt the user picked — the "Add a new prompt" choice is
handled in its own section, so you should already have branched away there.)

Read the selected file's **body** — everything below the closing `---` of the
frontmatter — and treat it as the user's request for this turn. Carry it out
directly in this session.

Do not summarize or restate the prompt before acting, and do not ask "should I
proceed?" unless the prompt body itself calls for confirmation. The body is
authoritative; run it as written.

## Adding a new prompt (`/add-prompt`)

This skill is meant to grow. `/add-prompt` (or an explicit request to save/add a
new favorite prompt) runs this wizard: collect the details, then write the file
into `prompts/` so it shows up in the menu automatically — no manual editing.

### Step A — collect the details

If `$ARGUMENTS` already holds the name (e.g. `/add-prompt refactor`), use it.
Otherwise ask the user, in one short message, for whatever is missing:

- **name** — the filename and the `/<name>` shortcut. Lowercase, hyphenated, no
  spaces (e.g. `refactor`, `write-tests`).
- **title** — short menu label (e.g. `Refactor code`). If omitted, derive it from
  the name.
- **description** — one line shown under the title in the menu.
- **body** — the prompt text to reuse. Accept a paste; you may tidy formatting
  but keep the user's intent and wording.

Then ask the one structured choice via **AskUserQuestion** (single-select):

- "Also create a direct `/<name>` shortcut?" → options: **Yes (recommended)**,
  **No, menu only**.

### Step B — validate

- Glob `${CLAUDE_SKILL_DIR}/prompts/*.md`. If `<name>.md` already exists, tell
  the user and ask for a different name (or cancel).
- Normalize the name to lowercase with hyphens.

### Step C — write the prompt file

Write `${CLAUDE_SKILL_DIR}/prompts/<name>.md` with this shape:

```
---
title: <title>
description: <description>
shortcut: <name>
---

<body>
```

### Step D — (optional) write the direct-shortcut command

If the user chose Yes: find the `commands/` directory that already holds the
prompt-shortcut shortcuts — try `${CLAUDE_SKILL_DIR}/../../commands/` first; if
that path does not exist, use `~/.claude/commands/`. Copy the one-line format of
the existing `continue.md` (it tells this skill to run the `<name>` prompt
directly) and save it as `<name>.md` there.

### Step E — confirm

Tell the user the prompt is saved and will appear next time they run
`/prompt-shortcut` (also `/prompt-shortcut <name>`, and `/<name>` if a shortcut
was created). If the skill lives in a git repo (for example the cwd is the
`prompt-shortcut` project), offer to commit and push the new file.

## Notes

- This skill carries its `prompts/` folder with it, so it behaves identically
  across global, project, and plugin/marketplace installs.
- Shortcut commands in `commands/` only tell this skill which prompt to run; the
  prompt text itself lives only in `prompts/` (single source of truth).
- If `$ARGUMENTS` or a shortcut names a prompt that does not exist, surface the
  available titles instead of guessing.
