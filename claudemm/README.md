# ClaudeMM plugin for Claude Code

Packages ClaudeMM (the Windows mind-map organiser for Claude Code sessions)
as a Claude Code plugin. The folder above this one (`plugin/`) is the
**marketplace** `chronoscan`; this folder is the plugin `claudemm`, so the
installed id is `claudemm@chronoscan`. Three integration surfaces, all
pointing at `bin/ClaudeMM.exe` through `${CLAUDE_PLUGIN_ROOT}`:

| Surface | File | What it does |
|---|---|---|
| Hooks | `hooks/hooks.json` | SessionStart / UserPromptSubmit / Stop / Notification / SessionEnd run `ClaudeMM.exe --hook`, which forwards the event to the running window so sessions pulse live in the map. A few milliseconds each; a no-op when the window is closed. |
| MCP server | `.mcp.json` | `ClaudeMM.exe --mcp` exposes the session index to Claude: `list_sessions`, `search_sessions`, `get_session`, `list_projects`, `list_tags`, `set_tags`, `rename_session`, `set_note`, `show_in_map`, `open_session`, `rescan`, `web_servers` (the projects' dev servers: list with live state, start, stop, open). Inside Claude Code the tools are named `mcp__plugin_claudemm_claudemm__<tool>`. |
| Commands | `commands/mm.md`, `commands/tag.md` | `/claudemm:mm` shows the current chat in the map; `/claudemm:tag Urgent, Fiscal` tags it (no arguments = Claude picks the tags). Every tool that takes a session id also accepts `"current"` = the chat the tool is called from, resolved from the hooks' trail in `%APPDATA%\ChronoUI\claudemm_live.tsv`. |

`bin/` is filled by the ClaudeMM build (CMake post-build step copies
`ClaudeMM.exe` and `ChronoUI.dll` there); it is not checked in.
`.claude-plugin/plugin.json` is generated from `plugin.json.in` so its
version always equals `CLAUDEMM_VERSION` in `ClaudeMM/CMakeLists.txt`.

## Install

* **From the ClaudeMM installer**: tick "Register with Claude Code" (on by
  default). It runs `ClaudeMM.exe --register-plugin`, which calls the
  `claude` CLI: `claude plugin marketplace add <app>\marketplace` and
  `claude plugin install claudemm@chronoscan`. The uninstaller runs
  `--unregister-plugin`.
* **From the ClaudeMM window**: the "Claude Code plugin" toolbar button does
  the same, and turns amber when Claude Code's cached copy is older than
  the running exe (click to update).
* **By hand**:

  ```bash
  claude plugin marketplace add M:/ChronoScan/v3/root/ChronoUI/ClaudeMM/plugin
  claude plugin install claudemm@chronoscan
  ```

* **One session only, for development**:

  ```bash
  claude --plugin-dir M:/ChronoScan/v3/root/ChronoUI/ClaudeMM/plugin/claudemm
  ```

Claude Code copies the plugin into `~/.claude/plugins/cache/chronoscan/claudemm/<version>`
and only refreshes it when the version grows, so bump `CLAUDEMM_VERSION`
for every build you hand to someone, then re-run the registration (or
`claude plugin update claudemm@chronoscan`).

## Notes

* Windows only (the exe is Win32 / Direct2D). The MCP server, the hook
  forwarder and the registrar are the same exe in different modes.
* User data (tags, map organisation, window state) stays in
  `%APPDATA%\ChronoUI\*.tsv`; nothing under `.claude` is written by the plugin
  itself (Claude Code writes its own plugin records there).
* If you previously clicked "Live hooks" in the ClaudeMM window, the same hooks
  are also in `~/.claude/settings.json`. Both fire; the window handles the
  duplicates, and the plugin button offers to remove the old copies.
