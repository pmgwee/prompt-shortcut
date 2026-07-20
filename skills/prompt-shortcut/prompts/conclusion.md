---
title: Conclusion
description: Session-scoped wrap-up — only THIS session's execution, features, and setup steps.
shortcut: conclusion
---

Give me a tight, highlighted conclusion of what was done in **this session only** — the work that actually happened in the current conversation (your edits, commands, and outputs since this session started). Keep it short and scannable: bullets, no filler.

**Scope is strict, and this is the most important rule: conclude only THIS session's work, not the project.** This same project directory is worked on across multiple sessions, so the repo holds everyone's cumulative work — do NOT summarize that. Specifically, do NOT describe the project at large, its pre-existing features or architecture, the repository's git history, or anything done in another session. Do NOT run `git log` / `git diff` or enumerate repo files to fill space. Anchor only on what occurred in this session's own exchanges. If something already existed when this session began, it is out of scope — even if it is relevant to the project.

Cover exactly these three sections, each scoped to this session:

1) **Execution** — what you accomplished / did *in this session*, as bullet points (the actual work performed in these exchanges).
2) **Features** — what features or changes were implemented *in this session* — new only, not pre-existing project features.
3) **Setup** — the step-by-step actions I need to take to set up and run *what this session produced* (not the whole project).

Lead each section with the most important items. If a section has nothing for this session, write "none this session" rather than padding it with project-level content.
