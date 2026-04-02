---
date: 2026-04-02
status:
type: issue
tags:
  - claude
---
**Problem:** The Bash tool in Claude Code requires a POSIX shell (bash/zsh) to run commands. On Windows, there's no built-in POSIX shell, so it was failing with "No suitable shell found." As a result I was not able to read empty folders to understand how my vault's folder structure was set up as glob only grabs files so empty folders do not show up.

**Root cause:** Two settings in [~/.claude/settings.json](vscode-webview://0dg733uhjaivlcv9ask4lgoo4ifh183v4pv7cta8sshtgdpave6o/index.html?id=3c441ca2-359b-4f32-a6f6-adfcc853a3a2&parentId=1&origin=de0da0ea-c799-4581-990a-3294d0d14953&swVersion=4&extensionId=Anthropic.claude-code&platform=electron&vscode-resource-base-authority=vscode-resource.vscode-cdn.net&parentOrigin=vscode-file%3A%2F%2Fvscode-app&session=bd74d91d-0037-4d57-9765-494dc14a4e56) were pointing to the wrong Git Bash path:

- `env.SHELL`
- `CLAUDE_CODE_GIT_BASH_PATH`

Both were set to `C:\Program Files\Git\bin\bash.exe` — the default install location — but your Git was installed to `C:\Users\L051535\AppData\Local\Programs\Git\bin\bash.exe` instead (a per-user install rather than system-wide).

**Fix:** Updated both values in `~/.claude/settings.json` to the correct path, then restarted VS Code so the new env vars loaded.

**Why it matters:** Without a working shell, the `obsidian-cli` commands can't run, meaning I can only read vault files with the `Read` tool and can't list folders — which is why empty folders were invisible to me.