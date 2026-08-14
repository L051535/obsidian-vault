---
date: 2026-08-14
status:
type:
tags:
  - "#claude/mcp"
---
**CLI (Claude Code)** reads from `C:\Users\L051535\.claude.json`, root-level `mcpServers` key. That key currently lists exactly: **veeva-mcp, tavily, playwright, context7, task-master-ai, markdownify, goose, ms365** — the same 8 servers, all loaded globally for _every_ CLI session by default, which is why it blew the 200k budget from the start.

**This desktop app** has its own separate file, `%APPDATA%\Claude\claude_desktop_config.json` — but that one only lists `veeva-mcp`. Everything else I have here (Figma, monday.com, ms365, Azure DevOps, PubMed, etc.) isn't coming from that file at all — it's coming from a different connector-management system built into this app (I have tools for it: `list_connectors` / `search_mcp_registry`). Let me check what that shows.