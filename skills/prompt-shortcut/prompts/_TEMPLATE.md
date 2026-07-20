---
title: <Short label shown in the menu>
description: <One line — what this prompt does; shown under the title in the menu>
shortcut: <optional slash name, e.g. my-prompt. If set, also copy commands/continue.md to commands/<shortcut>.md>
---

<The prompt body goes here. This text is injected and executed when the user
picks this entry from the /prompt-shortcut menu, runs /prompt-shortcut <filename>,
or invokes the matching shortcut command. Write it as a direct instruction to
the agent — it becomes the user's request for that turn.>

## To add this prompt

1. Copy this file to `prompts/<name>.md` (name = the filename you want; the
   `shortcut` field can be shorter).
2. Fill in `title`, `description`, and optional `shortcut`.
3. Write the prompt body above (delete this "To add" section).
4. It now appears in `/prompt-shortcut` automatically.
5. (Optional) For a direct `/<shortcut>` command, copy an existing file in
   `commands/` and change the prompt name it references.
