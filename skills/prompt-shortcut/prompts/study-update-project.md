---
title: Study & update project
description: Read every file in the repo to learn it fully, then sync its README so you hold the full picture for the next request.
shortcut: study-update-project
---

Read the ENTIRE codebase of the current project — every source, config, and doc file, not just the files a grep/glob or keyword search happens to hit — so you build a complete and accurate model of the project before doing anything else.

Why this matters: targeted searches miss things. Config lists, constants, state files, data flow, and conventions often live only inside code comments and never surface to a keyword query. Reading the whole tree is the only way to actually hold the full picture; guessing from filenames or a snippet is not enough.

Steps:
1. **Enumerate everything.** List every file in the repo (skip only heavy noise — `node_modules/`, `.venv/`/`venv/`, `dist/`/`build/`/`target/`, `.git/`, lockfiles, generated/minified output). Use `git ls-files` or Glob across the whole tree, not just the paths a keyword match returns.
2. **Read each file in full.** Read every source/config/doc file end-to-end, not only the lines around a grep match. For very large generated files, read the parts that matter. You may fan out to parallel subagents to read in breadth, but every file must actually be read — never infer a file's contents from its name or a fragment.
3. **Hold the full picture:** components and their responsibilities, entry points, data flow, external integrations, config/env vars, state/persistence, schedules/cron/CI, conventions, gotchas — anything a maintainer needs to know.

Then **update the repo's README** (and per-component READMEs if present) so they reflect the CURRENT system, not a stale summary:
- Correct anything inaccurate or missing.
- Include the concrete operator reference a developer actually needs — config lists, key constants, env-var map, state files, schedules — not just high-level prose. The README should be usable as a reference without having to open source files.
- For any values copied from code, note the source file and that the code is the source of truth, so the duplication is honest about drift.
- Match the existing doc style. Don't make changes beyond making the docs accurate and complete.

When finished, give me a concise summary: the architecture (what the project is, its components, where things run), what was stale or missing in the docs and what you changed, and any inconsistencies or gaps worth flagging.

You now hold the full project context. My next message will be the real question or requirement — solve it with the whole codebase in mind.
