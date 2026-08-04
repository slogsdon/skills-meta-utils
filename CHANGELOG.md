# Changelog

## 0.2.0 — 2026-08-04

**Removed `compress-prompt`.**

**Why:** a prompt-and-skill audit over 539 sessions (2026-04-03 → 2026-08-04)
recorded a single invocation, on 2026-05-06. The skill delegated text
shortening to a local model — a prompt with a model flag, not a capability
worth a trigger slot.

**Where the content went:** the Obsidian vault, as
`Knowledge/Reference/AI Prompts/Compress Prompt`. Full text also remains in
this repo's git history at `fc4baef` and earlier. `qwen-executor` still covers
local delegation if you want the same thing done by a model.

**Also documented:** `write-a-skill` was already shipping but missing from the
README and the plugin description. Both now list it.

## 0.1.0

Initial release — compress-prompt and qwen-executor.
