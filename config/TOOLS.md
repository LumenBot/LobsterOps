# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## Infrastructure

### VPS (Primary Host)
- **Name:** openclaw-lab
- **IP:** 68.183.5.172
- **OS:** Linux 6.8.0-71-generic (x64)
- **Node:** v22.22.0
- **Location:** /root/.openclaw/

### Workspace Structure
```
/root/.openclaw/
├── workspace/              # Ralph (main agent)
│   ├── AGENTS.md          # Operating manual
│   ├── SOUL.md            # Personality
│   ├── USER.md            # Blaise preferences
│   ├── TOOLS.md           # This file
│   ├── HEARTBEAT.md       # 2min periodic checks
│   ├── MEMORY.md          # Long-term curated memory
│   ├── memory/            # Daily logs, state files
│   ├── vault/             # ClawVault primitives
│   ├── research/          # Research documentation
│   └── skills/            # Custom skills
├── workspace-constituent/ # The Constituent agent
├── workspace-shared/      # Inter-agent coordination
│   ├── to-ralph/         # Messages for Ralph
│   ├── to-constituent/   # Messages for Constituent
│   └── archive/          # Processed messages
└── logs/                 # Gateway logs
```

## Multi-Agent Setup

### Agents
- **Ralph** (main): Orchestrator, veille, research, coordination
  - Session: agent:main:main
  - Model: claude-sonnet-4-5 (200k context)
  - Heartbeat: 2min
  - Workspace: /root/.openclaw/workspace/
  
- **The Constituent** (constituent): Constitutional specialist
  - Session: agent:constituent:main  
  - Model: claude-sonnet-4-5 (200k context)
  - Heartbeat: Weekly Sunday (standby mode)
  - Workspace: /root/.openclaw/workspace-constituent/

### Coordination Protocol (Canal Direct)
- **Messages to Ralph**: workspace-shared/to-ralph/YYYY-MM-DD-HHMM-[slug].md
- **Messages to Constituent**: workspace-shared/to-constituent/YYYY-MM-DD-HHMM-[slug].md
- **Archive**: workspace-shared/archive/ (processed messages)
- **Check frequency**: Ralph 2min, Constituent weekly

## Commands

### OpenClaw
```bash
openclaw status              # Gateway + agents status
openclaw gateway restart     # Restart gateway service
openclaw logs --follow       # Live gateway logs
openclaw security audit      # Security check

# ⚠️ SIGUSR1 — TOUJOURS utiliser nohup pour éviter que le signal tue le shell
nohup kill -USR1 $(pgrep -f openclaw-gateway) &   # ✅ Correct
kill -USR1 $(pgrep -f openclaw-gateway)            # ❌ Signal remonte dans le process exec → abort
```

### GitHub CLI
```bash
gh repo view                 # View repo info
gh issue list                # List issues
gh pr list                   # List PRs
gh run list                  # List CI runs
```

### ClawHub (Skill Marketplace)
```bash
clawhub search [query]       # Search skills
clawhub install [skill]      # Install skill
clawhub list                 # List installed skills
```

### Git (Workspace)
```bash
cd /root/.openclaw/workspace
git status                   # Check workspace changes
git add . && git commit -m "..." && git push  # Commit + push
```

## Credentials

**All credentials stored as environment variables** (never in files):
- `ANTHROPIC_API_KEY` — Claude API key
- `BRAVE_API_KEY` — Web search API key
- Other API keys as needed

**Never log or expose credentials**. Redact in all outputs.

## Integrations

### Channels
- **Telegram:** Primary interface
  - Ralph: Bot token configured
  - The Constituent: Separate bot (8215708120)
  - Reactions: MINIMAL mode (use sparingly)

### External Services
- **Brave Search API:** Web search, rate limited (2000/month)
- **GitHub API:** Via gh CLI, personal access token
- **ClawHub:** Skill marketplace, community repository

## Skills

### Installed Skills (Ralph)
- **clawvault:** Knowledge graph, compound learning (22 docs, 107 nodes)
- **weather:** Current weather, forecasts (no API key required)
- **github:** gh CLI wrapper (issues, PRs, CI)
- **tmux:** Remote-control tmux sessions
- **coding-agent:** Background process control (Codex, Claude Code)
- **skill-creator:** Create/update AgentSkills
- **healthcheck:** Security hardening, risk tolerance config

### Installed Skills (The Constituent)
- **constitution:** 31 articles tracked, version control
- **citizen:** Registry management (L2 approval required)
- **governance:** Proposal tracking (L2 activation required)

## ClawVault

### Structure
```
vault/
├── tasks/          # Active tasks, queued work
├── decisions/      # Strategic decisions, rationale
├── lessons/        # Mistakes, learnings, preventions
├── projects/       # Project documentation
└── people/         # People profiles, preferences
```

### Metrics (2026-02-16)
- **Documents:** 22
- **Knowledge graph:** 107 nodes, 101 edges
- **Compound learning:** ACTIVE
- **Trajectory:** Year 1 → 1,000 lessons → 10× productivity

### Key Files
- `vault/people/blaise.md` — Blaise profile
- `vault/projects/lobsterops.md` — LobsterOps documentation
- `vault/decisions/2026-02-16-strategic-pivot-ralph-priority.md` — Strategic pivot decision

## Notes

### Security
- Gateway: localhost only (127.0.0.1:18789)
- Auth: Token-based
- Skill vetting: Manual audit mandatory before installation
- File access: Workspace boundaries enforced

### Performance
- Model: claude-sonnet-4-5 (primary)
- Context: 200k tokens (monitor usage)
- Heartbeat cost: ~$0 (system messages)
- Multi-agent coordination: Zero cost (file-based)

### Monitoring
- Token usage: Check session_status
- Gateway health: openclaw status
- Coordination backlog: ls workspace-shared/to-ralph/ | wc -l
- Security alerts: memory/openclaw-security-log.md

---

_This is your cheat sheet. Update as infrastructure changes._
