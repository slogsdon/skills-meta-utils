---
name: write-a-skill
description: Guide writing a new skill file following Shane's authoring conventions — principle-first, anti-patterns before steps, _lib/ references over inline duplication. Use when invoked as /write-a-skill or when Shane says "create a new skill for X", "add a skill", "write a skill that does X".
---

# Skill: /write-a-skill [name]

A skill is a short, opinionated document — write the minimum that removes ambiguity, then stop.

**Don't:** embed blocks that appear in other skills — extract them to `_lib/`. Don't lead with numbered steps before stating what the skill is *for*.

## Conventions

**Length targets:**
- Simple delegation skill (Qwen, one task): under 40 lines
- Complex ritual skill (multi-step, fallback): under 80 lines
- If over limit, find what to extract to `_lib/`

**Structure (in order):**
1. Frontmatter: `name`, `description` (include trigger phrases and "Do NOT use for" guards)
2. H1 with skill name
3. **Principle line** — one sentence stating what the skill produces and why it's valuable; not "this skill does X" — just the thing itself
4. **Anti-patterns** — 1–3 `**Don't:**` lines naming specific failure modes and the right alternative
5. Steps or happy path
6. Fallback (if tool-dependent) — kept short via `_lib/` references

**Principle line pattern:**
Combine WHAT and WHY into one sentence. Examples:
- `/connect`: "Surface the non-obvious bridges between two concepts the vault hasn't linked yet — the value is in connections that surprise, not the ones you'd expect."
- `/trace`: "Map the arc of how Shane's thinking on a topic evolved — not the current position, but the path that led there and the inflection points along the way."

**`_lib/` candidates:**
- Any block >10 lines that appears in more than one skill
- Tool invocation protocols (Qwen delegation loop, obsidian commit sequence)
- Save/commit sequences

## Process

1. Determine the skill name and its trigger phrases.
2. Write the `description` frontmatter: trigger phrases + "Do NOT use for..." guard.
3. Write the principle line — one sentence, WHAT + WHY combined.
4. Write 1–3 anti-patterns using the `**Don't:**` format; name the right alternative.
5. Write steps — collapse any step that just restates what to do without showing how. If the skill depends on an external tool (Qwen, MCP), add a Fallback section.
6. Identify `_lib/` candidates — any block you've seen in another skill.
7. Count lines. If over target, extract one more thing or delete it.
8. Save to `plugins/shane/<plugin>/skills/<name>/SKILL.md`.
