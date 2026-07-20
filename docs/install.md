# Installing `prompt-shortcut` (agent-readable)

> You are an agent. A user pasted a one-line instruction that pointed you at this
> URL so you can install the **prompt-shortcut** skill for them. Follow these
> steps. Ask before doing anything destructive, and report real command output.

`prompt-shortcut` is a personal prompt launcher: `/prompt-shortcut` opens a
clickable menu of the user's favorite prompts, and `/continue`, `/conclusion`,
`/packaging` jump straight to each.

Repo: https://github.com/pmgwee/prompt-shortcut

## Step 1 — ask the user two questions

1. **Scope** — install globally (available in every project, recommended) or into
   the current project only?
   - Global → target `~/.claude/` (Windows: `%USERPROFILE%\.claude\`)
   - Project → target `./.claude/` in the current repo
2. If project scope: confirm which project directory.

Default to **global** if the user says "whatever" / "you pick".

## Step 2 — check prerequisites

- `git` available? (needed for clone; if not, fall back to the ZIP download in Step 3b)
- The target `.claude/` directory is writable.

Report what you found.

## Step 3 — get the files

Pick one:

**3a. Clone (preferred):**

```bash
# bash / macOS / Linux / Git Bash
git clone https://github.com/pmgwee/prompt-shortcut.git /tmp/prompt-shortcut
```

```powershell
# PowerShell
git clone https://github.com/pmgwee/prompt-shortcut.git "$env:TEMP\prompt-shortcut"
```

**3b. Download ZIP (if no git):** download
`https://github.com/pmgwee/prompt-shortcut/archive/refs/heads/main.zip` and
unzip it.

## Step 4 — copy into place

Global scope example:

```bash
# bash
mkdir -p ~/.claude/skills ~/.claude/commands
cp -r /tmp/prompt-shortcut/skills/prompt-shortcut ~/.claude/skills/
cp /tmp/prompt-shortcut/commands/*.md ~/.claude/commands/
```

```powershell
# PowerShell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills", "$env:USERPROFILE\.claude\commands" | Out-Null
Copy-Item -Recurse "$env:TEMP\prompt-shortcut\skills\prompt-shortcut" "$env:USERPROFILE\.claude\skills\"
Copy-Item "$env:TEMP\prompt-shortcut\commands\*.md" "$env:USERPROFILE\.claude\commands\"
```

For project scope, replace `~/.claude/` (or `$env:USERPROFILE\.claude\`) with
`<project>/.claude/`.

## Step 5 — verify

- Confirm the files exist: `~/.claude/skills/prompt-shortcut/SKILL.md` and four
  files under `~/.claude/commands/` (`continue.md`, `conclusion.md`, `packaging.md`, `add-prompt.md`).
- Tell the user to start a new Claude Code session (or run `/reload` if their
  client supports it) and type `/prompt-shortcut` — a menu with **Continue**,
  **Conclusion**, and **Packaging** should appear.

## Step 6 — report

Report exactly: scope chosen, where files were copied, the verification result,
and any errors. Do not claim success if any copy or check failed.

## Alternative: install as a plugin (no manual copy)

If the user prefers the plugin route (keeps everything in one bundle and gets
updates):

```
/plugin marketplace add pmgwee/prompt-shortcut
/plugin install prompt-shortcut@prompt-shortcut
```

Then `/prompt-shortcut` works the same way.
