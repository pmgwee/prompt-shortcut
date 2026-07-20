---
title: Packaging
description: Package a skill for distribution and install it (install-by-URL, manual copy, or marketplace).
shortcut: packaging
---

Help me package and install a Claude Code skill or plugin. If it is not obvious, first ask which skill or repo we are packaging and where its files live.

Then apply the recommended packaging method, and implement whichever parts I need for the current skill:

**Option A (recommended) — install by URL.** Ship a `docs/install.md` written for an agent to read from a raw GitHub URL, plus a one-line install instruction users paste to their own agent:

> Help me install <name>: https://raw.githubusercontent.com/<owner>/<repo>/main/docs/install.md

That `install.md` should: ask the user global-vs-project → check prerequisites → clone or download the ZIP → copy `skills/` and `commands/` into place → verify the skill appears → report real output.

**Option B (manual).** `git clone` (or on GitHub: Code → Download ZIP, then unzip), then copy:
- `skills/<name>/` into `~/.claude/skills/` (global) or `.claude/skills/` (this project)
- `commands/*.md` into `~/.claude/commands/` (global) or `.claude/commands/` (this project)

Give both bash and PowerShell commands.

**Also:** add a `.claude-plugin/plugin.json` manifest (and a `marketplace.json` if the repo should double as a marketplace) so the skill installs cleanly via `/plugin marketplace add <owner>/<repo>` then `/plugin install <name>`.

After implementing, summarize what you created and give me the exact install instruction I can share with others.
