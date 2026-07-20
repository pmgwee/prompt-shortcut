# prompt-shortcut

A personal **prompt launcher** for Claude Code. Type `/prompt-shortcut` and a
clickable menu of your favorite prompts pops up — pick one and it runs in the
current session. Distinct shortcuts (`/continue`, `/summarize`, `/package`) jump
straight to each prompt.

Designed to **scale**: every prompt is one file in `prompts/`. Drop a file in,
it shows up in the menu next time — no code edits.

---

## Install

### Option A — install by URL (recommended)

Paste this single line to your Claude Code agent:

```
Help me install prompt-shortcut: https://raw.githubusercontent.com/pmgwee/prompt-shortcut/main/docs/install.md
```

Your agent reads [`docs/install.md`](docs/install.md), asks you global-vs-project,
then clones and copies the files into place and verifies it.

### Option B — install as a plugin (official, keeps everything bundled)

```
/plugin marketplace add pmgwee/prompt-shortcut
/plugin install prompt-shortcut@prompt-shortcut
```

Updates come from the repo automatically.

### Option C — manual

Clone (or GitHub → **Code → Download ZIP**, then unzip):

```bash
# bash
git clone https://github.com/pmgwee/prompt-shortcut.git
cp -r prompt-shortcut/skills/prompt-shortcut ~/.claude/skills/
cp prompt-shortcut/commands/*.md ~/.claude/commands/
```

```powershell
# PowerShell
git clone https://github.com/pmgwee/prompt-shortcut.git
Copy-Item -Recurse prompt-shortcut\skills\prompt-shortcut "$env:USERPROFILE\.claude\skills\"
Copy-Item prompt-shortcut\commands\*.md "$env:USERPROFILE\.claude\commands\"
```

For a single project, copy into `<project>/.claude/` instead of `~/.claude/`.

Start a new session and run `/prompt-shortcut` to confirm.

---

## Usage

- `/prompt-shortcut` → opens the menu. Pick a prompt; it runs now.
- `/prompt-shortcut <name>` → run a specific prompt without the menu
  (e.g. `/prompt-shortcut summarize`).
- `/continue`, `/summarize`, `/package` → one-tap shortcuts for the built-ins.

The menu shows **buttons when there are 4 prompts or fewer**, and switches to a
**numbered list** as your library grows (AskUserQuestion caps at 4 buttons).

---

## The built-in prompts

| Shortcut | Title | What it does |
|----------|-------|--------------|
| `/continue` | Continue working | Finish all remaining tasks in the session without stopping between steps. |
| `/summarize` | Summarize outcomes | Bulleted wrap-up: Execution / Features / Setup steps. |
| `/package` | Package / install a skill | Package & install a skill via install-by-URL, manual copy, or marketplace. |

---

## Add a new prompt

1. Copy [`skills/prompt-shortcut/prompts/_TEMPLATE.md`](skills/prompt-shortcut/prompts/_TEMPLATE.md)
   → `prompts/<your-name>.md`.
2. Fill in the frontmatter:
   - `title` — menu label
   - `description` — one line under the title
   - `shortcut` — (optional) the `/<name>` you want, if any
3. Write the prompt body (this becomes the user's request when picked).
4. It now appears in `/prompt-shortcut` automatically.
5. **(Optional)** For a direct `/<shortcut>` command, copy any file in
   [`commands/`](commands) to `commands/<shortcut>.md` and change the prompt
   name it references.

That's it — no other files need editing.

---

## How it works

- **`skills/prompt-shortcut/SKILL.md`** is the menu. At runtime it enumerates
  every `prompts/*.md` in its own folder (via `${CLAUDE_SKILL_DIR}`), reads each
  file's frontmatter, and builds the menu. It carries the `prompts/` folder with
  it, so it works the same whether installed globally, per-project, or as a
  plugin.
- **`prompts/*.md`** is the single source of truth for prompt text.
- **`commands/*.md`** are thin shortcuts that tell the skill which prompt to run
  — the prompt text itself is never duplicated.

```
prompt-shortcut/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── skills/prompt-shortcut/
│   ├── SKILL.md
│   └── prompts/            ← drop files here to add prompts
│       ├── _TEMPLATE.md
│       ├── continue.md
│       ├── summarize.md
│       └── package.md
├── commands/               ← /continue  /summarize  /package
│   ├── continue.md
│   ├── summarize.md
│   └── package.md
├── docs/install.md
└── README.md
```

---

## License

MIT
