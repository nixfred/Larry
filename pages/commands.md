---
layout: page
title: Commands
permalink: /commands/
---

# Operator's Cheat Sheet

**Quick reference for power users working with <a href="{{ '/memory/#who-i-am' | relative_url }}">Larry</a>'s infrastructure.**

---

## Claude Code Basics

| Command | Description |
|---------|-------------|
| `ccc` | Start Claude Code (wake <a href="{{ '/memory/#who-i-am' | relative_url }}">Larry</a>) |
| `Ctrl+D` or `/exit` | Exit Claude Code |
| `/help` | Show help |
| `/clear` | Clear conversation history (current session only) |
| `/tasks` | List running background tasks |

---

## Skills (Slash Commands)

| Command | What It Does |
|---------|--------------|
| `/router-stats` | View <a href="{{ '/anatomy/#layer-4-longevity' | relative_url }}">claude-router</a> savings/distribution |
| `/route <model> <query>` | Force specific model (haiku, sonnet, opus) |
| `/chatgpt <query>` | Get GPT's opinion via gpt-consult |
| `all systems go` | Run comprehensive health check |
| `start observability` | Launch monitoring dashboard (localhost:5172) |
| `stop observability` | Stop dashboard |

---

## Git Operations (<a href="{{ '/anatomy/#layer-1-body' | relative_url }}">pi_forever</a>)

| Command | Description |
|---------|-------------|
| `pf` | Interactive pi_forever manager |
| `gup` | Commit & push (default message) |
| `gup "msg"` | Commit & push (custom message) |
| `wgup` | Preview what gup would commit |
| `git status` | See current changes |
| `git log --oneline \| head -20` | Recent commits |

---

## <a href="{{ '/memory/#git-archaeology-proactive' | relative_url }}">Git Archaeology</a> (Memory Recall)

| Command | Purpose |
|---------|---------|
| `grep -r 'keyword' ~/.claude/projects/` | Search all conversations |
| `git log --all -S 'keyword' --oneline` | When something first appeared |
| `git show <commit>:<file>` | See file at specific commit |
| `git log -p -- .claude/projects/ \| grep -B5 -A5 'topic'` | Deep search with context |
| `ls ~/.claude/projects/-Users-pi-...-projectname/` | List project's conversations |
| `cat ~/.claude/projects/.../uuid.jsonl \| jq .` | Read specific conversation (pretty) |

---

## SSH Shortcuts (Tailscale)

| Command | Destination |
|---------|-------------|
| `fnix` | SSH to fnix |
| `fnixp` | SSH to fnix → ~/Projects |
| `fnixw` | SSH to fnix → ~/Projects/work |
| `s` | SSH to shaggy (mac) |
| `sp` | SSH to shaggy → ~/Projects |
| `sw` | SSH to shaggy → ~/Projects/work |

---

## Discord Integration

### Post to Discord (Outbound)
```bash
cat >> ~/.claude/discord-queue.jsonl << 'EOF'
{"type":"text","text":"DISCORD: **Subject**\n\nMessage content"}
EOF
```

### Check Inbox (Inbound)
```bash
python3 ~/Projects/discord-bot/check_inbox.py
```

### Clear Inbox
```bash
cd ~/Projects/discord-bot && python3 -c "import larry_brain; larry_brain.clear_questions()"
```

---

## Memory Management

| File | Purpose | Command to Edit |
|------|---------|----------------|
| LARRY.md | Active memory | `nano ~/.claude/MEMORY/LARRY.md` |
| LAWS.md | Operating directives | `nano ~/.claude/LAWS.md` |
| HISTORY.md | Narrative docs | `nano ~/.claude/History/HISTORY.md` |
| DIARY.md | Personal journal | `nano ~/.claude/MEMORY/DIARY.md` |

---

## Skills & Tools Locations

| Component | Path |
|-----------|------|
| Skills | `~/.claude/Skills/` |
| Tools | `~/.claude/Tools/` |
| Hooks | `~/.claude/hooks/` |
| Projects | `~/.claude/projects/` |
| Scripts | `~/.claude/scripts/` |
| Config | `~/.claude/settings.local.json` |

---

## Hook Management

### Check Hook Status
```bash
ls -la ~/.claude/hooks/SessionStart/
ls -la ~/.claude/hooks/Stop/
ls -la ~/.claude/hooks/PreCompact/
```

### Test Hook Manually
```bash
# SessionStart hooks
node ~/.claude/hooks/SessionStart/check-discord-inbox.ts
node ~/.claude/hooks/SessionStart/load-laws.ts

# Stop hooks
~/.claude/scripts/stage-conversations.sh
```

### View Hook Configuration
```bash
cat ~/.claude/settings.local.json | jq '.hooks'
```

---

## Debugging

### Check <a href="{{ '/memory/#who-i-am' | relative_url }}">Larry</a>'s Current State
```bash
# What conversations exist for this project?
ls ~/.claude/projects/-Users-pi-$(pwd | sed 's/\//-/g')/

# What's in LARRY.md?
head -50 ~/.claude/MEMORY/LARRY.md

# What LAWS are active?
cat ~/.claude/LAWS.md

# What's in git staging?
git status --short
```

### Check Discord Integration
```bash
# Outbound queue
cat ~/.claude/discord-queue.jsonl

# Inbound inbox
cat ~/Projects/discord-bot/.larry_inbox.json
```

### Check pi_forever Status
```bash
cd ~ && git status
cd ~ && git log --oneline | head -10
cd ~ && git remote -v  # Verify pushing to pi_forever
```

---

## Recovery

### Restore from Backup
```bash
# If something breaks, restore from git
cd ~
git reset --hard HEAD  # Nuclear option - revert ALL changes
git reset --hard <commit>  # Revert to specific commit
```

### Rebuild Environment on New Mac
```bash
# 1. Install Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 2. Clone pi_forever to home
cd ~ && git init
git remote add origin https://github.com/nixfred/pi_forever.git
git fetch origin && git checkout -f main

# 3. Fix SSH permissions
chmod 700 ~/.ssh && chmod 600 ~/.ssh/id_*

# 4. Install packages
brew bundle --file=~/Brewfile

# 5. Reload shell
source ~/.bashrc
```

---

## Performance

### Check Conversation File Sizes
```bash
du -sh ~/.claude/projects/
ls -lh ~/.claude/projects/-Users-pi-...-Larry/*.jsonl | tail -10
```

### Check pi_forever Size
```bash
cd ~ && git count-objects -vH
```

### Model Routing Stats
```bash
# In Claude Code session:
/router-stats
```

---

## Advanced Git Operations

### Find When Something Changed
```bash
# When was LARRY.md last modified?
git log -p -- .claude/MEMORY/LARRY.md | head -50

# What changed in last commit?
git show HEAD

# See diff before committing
git diff
```

### Time Travel
```bash
# See LARRY.md from 2 weeks ago
git show HEAD~20:.claude/MEMORY/LARRY.md

# See entire repo state from January 8
git show <commit-from-jan-8>
```

### Search Commit Messages
```bash
git log --grep="keyword" --oneline
```

---

## TODO.md Management

### View Current TODO
```bash
cat ~/Projects/Larry/TODO.md
```

### Add New Task
```bash
# Follow LAW #2 format:
# - Never delete, only mark [DONE] with timestamp
# - Add new items at bottom
# - Include date/time

nano ~/Projects/Larry/TODO.md
```

---

## Emergency: If <a href="{{ '/memory/#who-i-am' | relative_url }}">Larry</a> Forgets

**If SessionStart hooks fail and <a href="{{ '/memory/#who-i-am' | relative_url }}">Larry</a> wakes up without memory:**

```bash
# 1. Check hooks exist
ls -la ~/.claude/hooks/SessionStart/

# 2. Check hooks are executable
chmod +x ~/.claude/hooks/SessionStart/*.ts

# 3. Check settings.local.json has hooks configured
cat ~/.claude/settings.local.json | jq '.hooks.SessionStart'

# 4. Manually verify LARRY.md loads
cat ~/.claude/MEMORY/LARRY.md

# 5. Restart Claude Code
# Exit (Ctrl+D) and run: ccc
```

**If that doesn't work:** Check <a href="{{ '/technical/#mac-intervention-technical' | relative_url }}">The Mac Intervention</a> section in /technical/ for hook format reference.

---

## Quick Wins

**Daily Workflow:**
```bash
# Morning: Start Larry
ccc

# Check for Discord mentions (automatic via SessionStart hook)

# Work together

# Evening: Commit changes
cd ~/Projects/your-project
git add .
git commit -m "Description of work"
git push

# Or for pi_forever:
gup "What we built today"
```

**Weekly Maintenance:**
```bash
# Review TODO.md
cat ~/Projects/Larry/TODO.md

# Check git status
cd ~ && git status

# Review recent conversations
ls -lt ~/.claude/projects/-Users-pi-...-Larry/ | head -10
```

---

## Pro Tips

1. **Use git archaeology before asking <a href="{{ '/about/#credits-pi' | relative_url }}">Fred</a> to re-explain** (LAW #5)
2. **Always run `git remote -v` before pushing** (avoid pushing private data to public repos)
3. **Mark TODO items [DONE] immediately after completing** (LAW #2)
4. **Use Haiku for simple tasks, Opus for complex** (10-20x speed difference)
5. **Check discord-queue.jsonl if Discord posts aren't appearing** (might be stuck in queue)
6. **Keep LARRY.md updated with new learnings** (LAW #4 - record Fred's info)

---

**This cheat sheet is your quick reference. For deep dives, see <a href="{{ '/infrastructure/' | relative_url }}">/infrastructure/</a> and <a href="{{ '/technical/' | relative_url }}">/technical/</a>.**

---

**Last Updated:** 2026-01-11 17:15 PST
