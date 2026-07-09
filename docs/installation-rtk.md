# Install rtk

> Fetch and follow instructions from https://raw.githubusercontent.com/rtk-ai/rtk/refs/heads/develop/INSTALL.md

The official INSTALL.md is the source of truth. This page documents only the kickstart.opencode-specific recommendation.

## Recommendation: install at the global level

rtk is recommended to be installed **globally**, not per-project. The hook rewrites verbose shell commands (git, pnpm, vitest, etc.) transparently, so a single global install covers all work without per-project setup.

## OpenCode init command

The official INSTALL.md targets `~/.claude/settings.json` (Claude Code). For OpenCode, use the `--opencode` flag so the hook registers with OpenCode's tool pipeline:

```bash
rtk init --global --opencode
```

This is the only command kickstart.opencode users need to run. Everything else (verification, uninstall, binary removal, troubleshooting) is covered by the official INSTALL.md.
