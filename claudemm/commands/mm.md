---
description: Show this chat in ClaudeMM (the session mind map) and bring the window to the front
argument-hint: [session id]
allowed-tools: Bash("${CLAUDE_PLUGIN_ROOT}/bin/ClaudeMM.exe" *)
---
Show this conversation in ClaudeMM.

1. Call the `show_in_map` tool of the `claudemm` MCP server. If `$ARGUMENTS` contains a session id, pass it as `id`; otherwise pass the current working directory as `cwd` (ClaudeMM then selects the most recent chat of this project, which is this one).
2. Only if that tool is unavailable, run this fallback with Bash instead:

```bash
"${CLAUDE_PLUGIN_ROOT}/bin/ClaudeMM.exe" --here "$(pwd -W 2>/dev/null || pwd)"
```

Then reply with one short line confirming ClaudeMM was asked to show this chat. Do nothing else.
