# AiSDK

Claude via the Agent SDK, in a native Sublime Text conversation view.
Extracted from `dpc00/SText` (`9cfa988`, 2026-07-31) after it was deleted
instead of being split out. GhostShell was extracted the same way.

This is **not** tommo/`sublime-claude`. It is the `Ai (SDK)` plugin that
lived in SText as `ai/ai_sdk.py` plus `backend/`. An attempt to mimic sublime-claude (ClaudeCode), not complete, many ommissions

## What it is

A headless Claude Code **agent loop** painted into an `Ai (SDK)` tab:

- `Ctrl+Alt+A` — open / focus the view
- `Ctrl+Alt+X` — restart the bridge (clears history)
- Enter — submit
- TCP `127.0.0.1:9503` — MCP eval into Sublime
- TCP `127.0.0.1:9504` — persistent `ClaudeSDKClient`

## Known holes (why it felt incomplete)

These are Agent SDK limits plus a local allowlist, not missing files:

1. **No Claude CLI slash commands.** The SDK is not the Ink TUI. `/model`,
   `/mcp`, `/config` have nowhere to run. `/clear` and `/cls` in this plugin
   are local view helpers, not CLI commands.
2. **Usual tools are thinned.** `backend/agent_query.py` comments out
   `Edit`, `Write`, `NotebookEdit`, `TodoWrite` (read-mostly lockdown after
   the agent wrote files unasked). Bash is on; the full CLI kit is not.

Everything else (native tab, bridge, MCP into ST, HIL approval, token
display) was already in the right place.

## Install

```powershell
New-Item -ItemType SymbolicLink -Path "$env:APPDATA\Sublime Text\Packages\AiSDK" -Target "<path to this checkout>"
```

Python 3.10+ with `claude-agent-sdk`, and `claude` on PATH. The plugin
still points at `C:\Users\donal\AppData\Local\Programs\Python\Python312\python.exe`.

## Layout

```
ai_sdk.py                 -- Sublime plugin (moved from SText/ai/ for package-root load)
backend/
  agent_query.py          -- ClaudeSDKClient bridge
  agent_query_ollama.py   -- Ollama-backed variant (off; _USE_OLLAMA is False)
  st_mcp_bridge.py        -- MCP stdio -> ST eval socket
  mcpclient/              -- MCP client used by the Ollama path
winutil/_job.py           -- Windows Job Object so the bridge dies with ST
```

Source snapshot: `SText@9cfa988`. Deletion (not authorized as a drop):
`SText@0a3cb2b` ("remove ai_sdk subsystem").
