---
name: qwen-executor
description: |
  Use this skill to delegate execution-level tasks to a local model via the LiteLLM proxy (default alias `fast-general`, resolving to gemma4:e4b-mlx). Best for: vault skill invocations (/ghost, /challenge, /emerge, /contradict, /drift, /ideas, /trace, /connect, /compound, /bloom, /stranger, /map, /level-up, /learned, /weekly-learnings, /backlinks), file search and summarization, drafting content, and any task where local execution is sufficient and API cost should be minimized. Do NOT use for tasks requiring real-time web access, complex multi-step tool use, or high-stakes decisions.
---

Delegate the current task to the local model (via the LiteLLM proxy on port 4000, default alias `fast-general`) using the stepped execution protocol below.

The target alias is configurable via the `OLLAMA_AGENT_MODEL` env var on the MCP server — see `~/Code/claude-code-config/models.yaml` for the full roster and aliases. The skill name retains "qwen" for historical continuity; the tool names (`qwen_start`/`qwen_continue`) are unchanged.

Do not attempt to answer the task yourself. Do not reason about it. Drive the loop immediately and return the final result verbatim.

## Tool Names

The Ollama-agent MCP server is registered under different namespaces depending on context:

- **Cowork (plugin context):** `mcp__plugin_shane-config_ollama-agent__qwen_start` / `mcp__plugin_shane-config_ollama-agent__qwen_continue`
- **Claude Code (standalone):** `mcp__ollama-agent__qwen_start` / `mcp__ollama-agent__qwen_continue`

Check which tools are available in your current session and use whichever namespace is present. If neither is available, use the fallback below.

## Stepped Execution Protocol

1. Call `qwen_start` (with the correct namespace prefix for your context) with `task`, `skill`, and `context` parameters.
2. Parse the JSON response:
   - `status: "done"` → return `result` to the user. Stop.
   - `status: "running"` → note the `session_id` and `step`, then call `qwen_continue` with that `session_id`.
   - `status: "error"` → surface the `result` as an error. Stop.
3. Repeat step 2 until status is `done` or `error`.

Each `qwen_continue` call is a separate tool call — you will see each step as it completes.

## Vault Access

The local model does **not** have access to MCP tools. For any task that requires reading or writing the Obsidian vault, the task description must include these instructions so the model knows how to interact with the vault:

```
Vault access (bash only — no MCP tools):
- Search: `obsidian search query='TERM' limit=10`
- Read:   `obsidian read file='Note Name'` (no .md extension)
- Write:  `obsidian append file='Note Name' content='TEXT'`
- Backlinks: `obsidian backlinks file='Note Name'`
- Daily note (read):  `obsidian daily:read`
- Daily note (write): `obsidian daily:append content='TEXT'`
```

This applies to all vault skills: /bloom, /ghost, /challenge, /emerge, /contradict, /drift, /trace, /connect, /compound, /stranger, /map, /level-up, /learned, /weekly-learnings, /backlinks. Each of those skills' task descriptions already includes this preamble.
