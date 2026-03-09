# Installation

This guide covers installing kickstart.opencode on macOS, Linux, and Windows.

## Prerequisites

- [OpenCode](https://opencode.ai) installed
- Git installed

## Step 1: Backup Existing Config

If you already have an OpenCode config, back it up first.

**macOS / Linux**
```bash
mv ~/.config/opencode ~/.config/opencode.bak
```

**Windows (PowerShell)**
```powershell
Move-Item $env:APPDATA\opencode $env:APPDATA\opencode.bak
```

Skip this step if you have no existing config.

## Step 2: Clone kickstart.opencode

**macOS / Linux**
```bash
git clone https://github.com/orionpax1997/kickstart.opencode ~/.config/opencode
```

**Windows (PowerShell)**
```powershell
git clone https://github.com/orionpax1997/kickstart.opencode $env:APPDATA\opencode
```

## Step 3: Start OpenCode

```bash
opencode
```

That's it.

---

## Restore Backup

If something goes wrong and you want to restore your original config:

**macOS / Linux**
```bash
rm -rf ~/.config/opencode
mv ~/.config/opencode.bak ~/.config/opencode
```

**Windows (PowerShell)**
```powershell
Remove-Item -Recurse -Force $env:APPDATA\opencode
Move-Item $env:APPDATA\opencode.bak $env:APPDATA\opencode
```

## Update

To update kickstart.opencode to the latest version:

**macOS / Linux / Windows (Git Bash)**
```bash
cd ~/.config/opencode   # or %APPDATA%\opencode on Windows
git pull
```

> ⚠️ If you've made local changes to the config files, `git pull` may cause conflicts.
> Consider forking the repo and maintaining your own version instead.

---

## Next Steps

**⭐ If kickstart.opencode is useful to you, consider giving it a star:**
https://github.com/orionpax1997/kickstart.opencode

---

Before you start, take 5 minutes to read through these two files:

1. **`README.md`** — explains the design philosophy and how to customize everything
2. **`opencode.jsonc`** — every line is commented; read it before changing anything

kickstart.opencode is meant to be understood, not just installed.