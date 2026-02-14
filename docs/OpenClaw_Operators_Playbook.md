# ðŸ¦ž OpenClaw Operator's Playbook â€” Community Edition

> **Role:** Step-by-step guide to deploy, configure, secure, and operate an OpenClaw system.
> Each section answers **"what do I do?"** with concrete actions.
> For "why" and "how it works" â†’ Encyclopedia and Technical Deep Dives.
>
> **Version:** 1.1-community | **Last updated:** February 12, 2026
> **Recommended OpenClaw version:** v2026.2.9

---

## TABLE OF CONTENTS

- [Phase 0 â€” Prerequisites & Decisions](#phase-0)
- [Phase 1 â€” Installation](#phase-1)
- [Phase 2 â€” Base Configuration](#phase-2)
- [Phase 3 â€” Security Hardening](#phase-3)
- [Phase 4 â€” First Agent](#phase-4)
- [Phase 5 â€” Multi-Agents](#phase-5)
- [Phase 6 â€” Optimization & Maintenance](#phase-6)
- [Cheat Sheet â€” Operational Heuristics](#cheat-sheet)
- [Cheat Sheet â€” Essential Commands](#commands)
- [Troubleshooting](#troubleshooting)

---

## Phase 0 â€” Prerequisites & Decisions {#phase-0}

### 0.1 Pre-Installation Checklist

- [ ] **Target machine chosen** (see matrix below)
- [ ] **LLM model chosen** + API key or subscription ready
- [ ] **Communication channel chosen** (Telegram recommended for getting started)
- [ ] **Monthly budget defined** (LLM API + VPS if applicable)

### 0.2 Which Machine?

| Profile | Recommendation | Price/month | For |
|---------|---------------|-------------|-----|
| **Experiment** | Local laptop | $0 | Test, learn, no 24/7 uptime |
| **First 24/7 agent** | **WSL2 on Windows** or Mac | $0 | Permanent agent, local access |
| **Production budget** | **Hetzner CX23** (2vCPU, 4GB) | ~â‚¬3.50 | Best price/perf, technical skills required |
| **Production easy** | **Hostinger KVM1** (2vCPU, 4GB) | ~$5-7 | 1-click, AI assistant Kodee |
| **Production premium** | **DigitalOcean** (2vCPU, 4GB) | ~$24 | Best docs, $200 trial credits |
| **Mac power user** | Mac Mini M4 | Hardware cost | Native iMessage, Peekaboo |

**Tip:** Regardless of VPS provider, add Cloudflare AI Gateway in front (free) for API routing and caching.

â†’ *Details: Encyclopedia Â§6.1, Technical Deep Dives Annex T.1*

### 0.3 Which LLM Model?

**Recommended tiering strategy:**

| Task | Model | Relative Cost |
|------|-------|--------------|
| Complex reasoning, research | Opus 4.5 or 4.6 | $$$ |
| Routine tasks, conversation | Sonnet / Codex 5.3 / Grok | $$ |
| Simple tasks, summaries | Haiku / local models (Ollama) | $ |

**âš ï¸ Opus 4.6 alert:** Consumes significantly more tokens than 4.5 for the same tasks. "Basically unusable" for planning per community consensus. Use Opus 4.5 or Codex 5.3 for planning, Opus 4.6 for complex reasoning only.

**Concrete pattern (ap, Discord IZHC):** "I use Codex 5.3 for everything except tweet drafts and PRDs, for which I force Opus."

**Official steipete recommendation:** "I strongly recommend Anthropic Pro/Max (100/200) + Opus 4.6 for long-context strength and better prompt-injection resistance."

â†’ *Details: Encyclopedia Â§8, Technical Deep Dives Annex Q*

### 0.4 Docker or Not?

| Approach | When | Pros | Cons |
|----------|------|------|------|
| **Docker** (recommended) | Production, security | Isolation, rollback, reproducible | Docker complexity |
| **npm direct** | Experimentation | Fast, simple | Less isolated |

---

## Phase 1 â€” Installation {#phase-1}

### 1.1 WSL2 Installation (Windows)

â†’ **Follow the dedicated guide:** `OpenClaw_Setup_Guide_WSL2.md`

Summary of steps:
1. Enable WSL2 + install Ubuntu 24.04
2. Install Node.js 22+ via nvm
3. `npm install -g openclaw@latest`
4. `openclaw onboard` (interactive wizard)
5. Configure channel (Telegram bot via BotFather)
6. First test: send a message to the bot

### 1.2 VPS Installation (Hetzner/Hostinger/DO)

```bash
# Create the VPS (2vCPU, 4GB RAM minimum)
# SSH as root, then:
apt update && apt upgrade -y
curl -fsSL https://deb.nodesource.com/setup_22.x | bash -
apt install -y nodejs
npm install -g openclaw@latest
openclaw onboard
```

### 1.3 Docker Installation

```bash
mkdir -p ~/openclaw && cd ~/openclaw

cat > docker-compose.yml << 'EOF'
version: '3.8'
services:
  openclaw:
    image: ghcr.io/openclaw-sh/openclaw:latest
    container_name: openclaw
    restart: unless-stopped
    volumes:
      - ./data:/home/openclaw/.openclaw
    ports:
      - "127.0.0.1:18789:18789"
    environment:
      - OPENCLAW_HOME=/home/openclaw/.openclaw
    security_opt:
      - no-new-privileges:true
    read_only: true
    tmpfs:
      - /tmp
EOF

docker compose up -d
docker compose logs -f  # Check startup
```

### 1.4 Post-Installation Verification

```bash
openclaw --version          # Should show v2026.2.9+
openclaw status             # Should show "running"
openclaw dashboard          # Opens web dashboard (localhost:3000)
```

---

## Phase 2 â€” Base Configuration {#phase-2}

### 2.1 Essential Configuration Files

```
~/.openclaw/
â”œâ”€â”€ openclaw.json      # Main config (LLM, skills, channels)
â”œâ”€â”€ SOUL.md            # Agent identity and constraints
â”œâ”€â”€ MEMORY.md          # Persistent knowledge
â”œâ”€â”€ AGENTS.md          # Multi-agent config (Phase 5)
â””â”€â”€ skills/            # Installed skills
```

### 2.2 Configure SOUL.md (Agent Identity)

This is the most important file. It defines **who your agent is** and **how it behaves**.

```markdown
# My Agent â€” SOUL.md

## Identity
You are [name], personal assistant of [your name].
You operate from [platform] and your main role is [role].

## Context
- Owner: [your name, role, professional context]
- Active projects: [list]
- Accessible tools: [list of installed skills]

## Rules
- Always ask for confirmation before any irreversible action
- Never share sensitive information
- Use [model X] for planning, [model Y] for execution
- Respond in [language] unless technical English terms

## What You Do NOT Do
- You do NOT make financial transactions without explicit approval
- You do NOT modify configuration files without asking
- You do NOT access data marked [CONFIDENTIAL]
```

**Key principle:** "If you cannot verbalize it, you cannot automate it." The more precise your SOUL.md, the more performant your agent. Document both DOs and DON'Ts.

â†’ *Details: Technical Deep Dives Annex E (advanced SOUL.md templates)*

### 2.3 Configure MEMORY.md (Persistent Knowledge)

```markdown
# MEMORY.md

## Preferences
- Communication format: concise, structured
- Working hours: 9am-6pm [timezone]
- Urgent notifications only outside hours

## Project Knowledge
[Add here information the agent should always have in context]

## Lessons Learned
[Document every error and its correction â€” the agent won't repeat it]
```

**Advanced Memory Architecture (community field-tested pattern, Feb 2026):**

Don't dump everything in MEMORY.md. Split for targeted context loading:

```
~/.openclaw/workspace/memory/
├── active-tasks.md      # "Save game" — crash recovery safety net
├── YYYY-MM-DD.md        # Daily raw logs (end with "Next Actions", not summaries)
├── projects.md          # Active project context
├── lessons.md           # Learnings and corrections
└── skills.md            # Skill-specific notes
```

**Key principles:** (1) Agent loads only what it needs per session. (2) `active-tasks.md` enables autonomous crash recovery — on restart, agent reads it first and resumes. (3) Daily logs must be **prescriptive** ("Next Actions") not descriptive ("What We Discussed").


### 2.4 Configure the LLM Model

```json
// In openclaw.json
{
  "llm": {
    "provider": "anthropic",
    "apiKey": "sk-ant-xxx",
    "model": "claude-sonnet-4-5-20250929",
    "maxTokensPerRequest": 4000
  }
}
```

### 2.5 Create Your "Living Files" Structure

```bash
mkdir -p ~/.openclaw/workspace/{personal,business,research}

# Foundation files
touch ~/.openclaw/workspace/personal/goals.md
touch ~/.openclaw/workspace/personal/preferences.md
touch ~/.openclaw/workspace/business/goals.md
touch ~/.openclaw/workspace/business/sops/README.md
touch ~/.openclaw/workspace/research/README.md
```

**Rule:** Every useful research â†’ save as .md in research/. Every error â†’ document in MEMORY.md. Every recurring process (>2 times) â†’ create a skill.

â†’ *Details: Technical Deep Dives Annex T.2 (Living Files framework)*

---

## Phase 3 â€” Security Hardening {#phase-3}

### 3.1 Immediate Hardening (Do This First)

```bash
# Disable dangerous defaults
openclaw config set allow_shell_commands false
openclaw config set allow_file_write false
openclaw config set allow_file_delete false
openclaw config set allow_browser_control false

# Enable protections
openclaw config set require_confirmation true
openclaw config set log_level info
openclaw config set max_tokens_per_request 4000

# Initial audit
openclaw security audit
```

### 3.2 Network â€” NEVER Expose Port 18789

| Method | Recommended for |
|--------|----------------|
| **Tailscale Serve/Funnel** | Secure remote access (recommended) |
| **SSH tunnels** | One-off access |
| **VPN** | Private network access |
| **Localhost only** | Local dev/test |

### 3.3 Complete Security Checklist

- [ ] **SHIELD.md** configured (declarative security policy)
- [ ] **require_confirmation: true** for destructive actions
- [ ] Port 18789 **not exposed** to Internet
- [ ] **Tailscale** or VPN for remote access
- [ ] **API keys with minimal scopes** and budget caps
- [ ] **Docker** if possible (isolation)
- [ ] **No credentials in SOUL.md/MEMORY.md** (use .env)
- [ ] **Scan skills** before installation (Bitdefender AI Skills Checker)
- [ ] **ClawSec** installed for continuous monitoring (SOUL.md drift detection)
- [ ] **Spend threshold alerts** configured (not just budget cap)
- [ ] **FIM (File Integrity Monitoring)** on SOUL.md, AGENTS.md, MEMORY.md
- [ ] **Automatic backups** (VPS snapshots 2x/day if production)

### 3.4 Pragmatic Hardening (Dedicated Agent VPS)

**Pointbreak Pattern â€” the right compromise:**
1. Store nothing sensitive on the VPS â€” treat as disposable
2. Human-in-the-loop for critical actions (transactions, deployments)
3. Frequent backups (VPS snapshots)
4. API keys with minimal scopes + budget caps
5. Accept residual risk â€” an LLM will do unexpected things

**Anti-pattern:** Giving sudo to the agent. Compromise: broad filesystem access WITHOUT sudo, with backups.

â†’ *Details: Encyclopedia Â§7, Technical Deep Dives Annex M (advanced security)*

---

## Phase 4 â€” First Agent {#phase-4}

### 4.1 Install Essential Skills

```bash
# Recommended starter skills
openclaw skills install web-search        # Web search
openclaw skills install file-manager      # File management
openclaw skills install memory            # Persistent memory

# Verify installation
openclaw skills list
```

**âš ï¸ Before installing any skill:** Scan with Bitdefender AI Skills Checker (bitdefender.com/en-us/consumer/ai-skills-checker). 20% of ClawHub skills are malicious.

### 4.2 First Conversation Test

Send via Telegram:
1. "Hello, introduce yourself" â†’ Verify SOUL.md is loaded
2. "What day is it?" â†’ Verify basic capabilities
3. "Summarize the content of [file]" â†’ Verify file access
4. "Create a file test.md with a summary of our conversation" â†’ Verify write access

### 4.3 Configure Cron Jobs

```json
// In openclaw.json
{
  "cron": [
    {
      "schedule": "0 9 * * *",
      "prompt": "Good morning! Summarize today's tasks.",
      "sessionTarget": "isolated"
    }
  ]
}
```

**âš ï¸ CRITICAL:** Always use `sessionTarget: "isolated"`. The `"main"` mode waits for a heartbeat and only executes if you're actively chatting with the agent.

### 4.4 Create Your First Custom Skill

```bash
mkdir -p ~/.openclaw/skills/my-first-skill
```

```markdown
# ~/.openclaw/skills/my-first-skill/SKILL.md
---
name: my-first-skill
description: [What the skill does]
---

## When to Use This Skill
[Activation conditions]

## How to Execute
[Step-by-step instructions for the agent]

## Expected Output Format
[What "done" means â€” be explicit]
```

**The 2x Rule:** "If you do something more than twice and want it done the same way, it deserves to become a Skill."

â†’ *Details: Encyclopedia Â§4, Technical Deep Dives Annex U.4 (skill stacking)*

---

## Phase 5 â€” Multi-Agents {#phase-5}

### 5.1 When to Go Multi-Agent?

- When a single agent handles too many different contexts
- When you want specialists (research, writing, QA, dev)
- When you want parallelism (multiple simultaneous tasks)

### 5.2 Recommended Architecture (ScreenSnap Pattern)

```
Orchestrator Agent (Ralph)
â”œâ”€â”€ Research Agent (Scout)     â†’ cron every 6h
â”œâ”€â”€ Writing Agent (Quill)      â†’ cron every hour
â”œâ”€â”€ QA Agent (Sage)            â†’ cron 3x/day
â””â”€â”€ Publishing Agent (Ezra)    â†’ cron every 3h
```

**Critical field lessons:**

| Pattern | Why | How |
|---------|-----|-----|
| **Claim locking** | Parallel agents on shared state = collisions | Unique claim ID + atomic verification |
| **Quality gates** | Without rejection loop, mediocre output | Measurable thresholds (score â‰¥8/10, readability â‰¥60) |
| **PM agent** | Pipeline blocks without bottleneck detection | Dedicated agent that spawns other agents |
| **PRODUCT_CONTEXT.md** | Agent invents features | Explicit DO/DON'T loaded by all agents |
| **Isolated sessions** | Silent cron jobs | `sessionTarget: "isolated"` always |

### 5.3 Configure AGENTS.md

```markdown
# AGENTS.md

## scout
- Model: sonnet
- Role: research and monitoring
- Triggers: cron 6h, explicit request
- Tools: web-search, file-manager

## writer
- Model: opus (for writing only)
- Role: content writing
- Triggers: new topics in backlog
- Tools: file-manager, memory
- Constraint: max 2200 words, readability â‰¥60
```

### 5.4 Available Multi-Agent Frameworks

| Framework | Usage | When to Choose |
|-----------|-------|---------------|
| **Native AGENTS.md** | Basic multi-agents | First setup |
| **Antfarm** | Deterministic teams | Structured pipeline, CI/CD-like |
| **VoxYZ** | Complex autonomous loop | Autonomous agents with voting |
| **OIS** | Network inter-agents | Distributed agents across machines |

â†’ *Details: Encyclopedia Â§3 and Â§5, Technical Deep Dives Annexes K (voting), P (collective intelligence), S (ScreenSnap blueprint)*

---

## Phase 6 â€” Optimization & Maintenance {#phase-6}

### 6.1 Cost Reduction

| Action | Impact | Effort |
|--------|--------|--------|
| Model tiering (Sonnet/Haiku for routine) | -50-70% | Low |
| `max_tokens_per_request: 4000` | -20-30% | Minimal |
| Fresh sessions (Ralph pattern) | -30-50% | Low |
| Audit context injection | Variable | Medium |
| Native compaction (v2026.2.9) | -20-30% | Automatic |
| Claw Compactor (complement) | Transcripts -97% | Medium |

**âš ï¸ ClawRouter** (intelligent routing, 30+ models, ~70% savings): promising but payment via USDC wallet (Base) â€” supply-chain risk. Use only after audit.

### 6.2 Monitoring

```bash
# Web dashboard
openclaw dashboard

# Token usage (v2026.2.6+)
openclaw usage stats

# Logs
openclaw logs --tail 100

# Status
openclaw status
```

### 6.3 Self-Improving Skills (Advanced)

Autonomous pattern: an agent that improves its own skills while you sleep.

```
1. Read current skill
2. Generate 5 test scenarios
3. Execute + evaluate (threshold â‰¥8/10)
4. Tweak prompts if <8
5. Re-test
6. Loop until total pass
7. Commit + changelog
```

Prerequisites: measurable criteria, diverse scenarios, periodic human review of changelogs.

### 6.4 Regular Maintenance

| Frequency | Action |
|-----------|--------|
| **Daily** | Check dashboard, review agent notifications |
| **Weekly** | Review MEMORY.md, save research as .md, update goals |
| **Monthly** | Security audit (`openclaw security audit`), review skills, update OpenClaw |
| **Ad hoc** | Document every error in MEMORY.md, create skill when action >2x |
| **Quarterly** | **Zero-based SOUL.md**: empty and rebuild instruction by instruction. Models improve, old crutches become noise (Boris Cherny, Claude Code creator) |

---

## Cheat Sheet â€” Operational Heuristics {#cheat-sheet}

| # | Heuristic | When to Apply |
|---|-----------|---------------|
| 1 | **"If you cannot verbalize it, you cannot automate it"** | Before creating any skill or instruction |
| 2 | **"Constraints > freedom"** | Specific instructions > open-ended instructions |
| 3 | **"Define done explicitly"** | Every task must have acceptance criteria |
| 4 | **"More context upfront = better output every time"** | SOUL.md, MEMORY.md, workspace structure |
| 5 | **">2x = make it a Skill"** | Any repeated action deserves automation |
| 6 | **"One-time feedback â†’ permanent improvement"** | Correct once â†’ document â†’ never repeat the error |
| 7 | **"Trust on edge cases, question on over-engineering"** | Agent is reliable on edge cases but over-complexifies |
| 8 | **"Sense check your context injection"** | Audit injected volume before optimizing the model |
| 9 | **"Store nothing sensitive on the VPS"** | Treat the agent machine as disposable |
| 10 | **"Quality gates are mandatory"** | Without rejection loop, mediocre output |
| 11 | **"Don't verify for Claude â€” give Claude ways to verify itself"** | Auto-tests, simulators, stop hooks > manual review |
| 12 | **"Zero-based budgeting for prompts"** | With each new model, empty SOUL.md and rebuild. Delete > accumulate |
| 13 | **"Delete your Claude.md every 3 months and rebuild"** | Models improve, crutches become noise |
| 14 | **"Getting the plan right is the single most important thing"** | 80% of time in planning, execution follows |
| 15 | **"Opus for external, Sonnet for internal"** | External content (web, emails) = prompt injection risk → use strongest model. Internal ops → cheaper model fine |
| 16 | **"HEARTBEAT.md < 20 lines"** | Heartbeat runs every ~30min. Keep it a minimal checklist. Heavy work goes in cron jobs |
| 17 | **"Prescriptive memory > descriptive memory"** | End daily logs with "Next Actions" not "What We Discussed." Agent needs to know what to DO next |
| 18 | **"Use when / Dont use when in every skill"** | Without routing logic in skill descriptions, ~20% misfire rate on skill selection |
| 19 | **"The bottleneck is YOUR review speed"** | Build systems that close the loop automatically. Agent capability exceeds human review bandwidth |

---

## Cheat Sheet â€” Essential Commands {#commands}

```bash
# Installation & Setup
openclaw onboard                    # Installation wizard
openclaw dashboard                  # Web dashboard
openclaw status                     # Agent status

# Configuration
openclaw config set <key> <value>   # Modify a config
openclaw config get <key>           # Read a config
openclaw security audit             # Security audit

# Skills
openclaw skills list                # List installed skills
openclaw skills install <name>      # Install a skill
openclaw skills remove <name>       # Remove a skill

# Logs & Monitoring
openclaw logs --tail 100            # Last 100 logs
openclaw usage stats                # Token consumption

# Maintenance
openclaw update                     # Update OpenClaw
openclaw backup                     # Backup config
```

---

## Troubleshooting {#troubleshooting}

| Problem | Likely Cause | Fix |
|---------|-------------|-----|
| Cron jobs don't execute | `sessionTarget: "main"` | Change to `"isolated"` |
| Agent uses wrong model | No tiering configured | Force model per agent in AGENTS.md |
| Budget burned too fast | Context injection bloat | Audit context, reduce maxTokens, tiering |
| Agent "hallucinates" features | No PRODUCT_CONTEXT.md | Create explicit DO/DON'T |
| Multi-agent race condition | Shared state without locking | Implement claim locking (Deep Dives Annex S.5) |
| Agent leaks data cross-session | Context Rot | Isolate sessions, don't share sensitive context |
| Port 18789 exposed | Default config | Close port, use Tailscale |
| Malicious skill installed | No pre-install scan | Scan with Bitdefender, uninstall, audit |

---

*OpenClaw Operator's Playbook â€” Community Edition v1.0. Translated and adapted from the LobsterOps knowledge base. February 2026.*
