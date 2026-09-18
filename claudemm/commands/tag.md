---
description: Tag this chat in ClaudeMM (the session mind map), e.g. /claudemm:tag Urgent, Fiscal
argument-hint: <tag>[, <tag>...] | -<tag> to remove
---
Tag this conversation in ClaudeMM.

1. Split `$ARGUMENTS` on commas into tags. A tag starting with `-` is a removal. If `$ARGUMENTS` is empty, choose one to three short tags yourself from what this chat is about (its topic, the project, and one of the presets In progress / Urgent / Don't forget / Waiting for reply / Done when it fits).
2. Call the `set_tags` tool of the `claudemm` MCP server with `id: "current"`, `add` = the tags to add and `remove` = the tags to remove. "current" resolves to this chat.
3. Reply with one short line listing the chat's tags as the tool returned them. Do nothing else.
