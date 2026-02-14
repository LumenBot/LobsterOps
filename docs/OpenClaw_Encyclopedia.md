# ðŸ¦ž OpenClaw Encyclopedia â€” Community Edition

> **Role:** Source of truth on OpenClaw architecture, concepts, and ecosystem.
> This document answers **"what is it?"** â€” stable, factual, comprehensive.
> For "what do I do" â†’ Operator's Playbook. For "what changed" â†’ Ecosystem Watch.
>
> **Version:** 1.1-community | **Last updated:** February 12, 2026
> **Origin:** Translated and adapted from the LobsterOps knowledge base for IZHC community use.

---

## TABLE OF CONTENTS

1. [Overview](#1-overview)
2. [Core Architecture](#2-core-architecture)
3. [Multi-Agent Routing & Orchestration](#3-multi-agent-routing--orchestration)
4. [Skills, Plugins & Ecosystem](#4-skills-plugins--ecosystem)
5. [Complementary Modules & Community Projects](#5-complementary-modules--community-projects)
6. [Deployment â€” Options & Architecture](#6-deployment--options--architecture)
7. [Security â€” Critical Points](#7-security--critical-points)
8. [Cost Optimization & Model Selection](#8-cost-optimization--model-selection)
9. [Success Stories & Use Cases](#9-success-stories--use-cases)
10. [Limitations, Critiques & Emerging Concepts](#10-limitations-critiques--emerging-concepts)
11. [Navigation to Other Documents](#11-navigation-to-other-documents)

---

## 1. Overview

**OpenClaw** is an open-source, self-hosted personal AI assistant framework created by **Peter Steinberger** (@steipete). It runs an autonomous AI agent on your own machines, connected to your existing messaging channels.

**Name history:**
- **Clawdbot** (Nov 2025) â†’ **Moltbot** (Jan 2026, Anthropic trademark request) â†’ **OpenClaw** (early 2026, definitive name)

**Key figures (Feb 2026):**
- ~185,000 GitHub stars, ~31,000 forks (as of Feb 12)
- 100,000 stars in 2 months — one of the fastest-growing GitHub projects ever
- 2 million visitors in one week at peak
- 5,705+ community skills on ClawHub
- 50+ platform integrations
- Wikipedia article now live (French, English)







**Tech stack:** TypeScript, Node.js, CLI + Gateway server

**Positioning:** Not a chatbot â€” a *controlled agent runtime* with file system, shell, browser, API, and messaging channel access. Sometimes described as a "libre Alexa" with autonomous execution capabilities.

**Supported channels:** WhatsApp, Telegram, Discord, Slack, Signal, iMessage, Google Chat, Microsoft Teams, WebChat, BlueBubbles, Matrix, Zalo, LINE, Feishu + voice (macOS/iOS/Android) + Canvas live.

---

## 2. Core Architecture

### 2.1 6-Stage Execution Pipeline

1. **Channel Adapter** â€” Standardizes inputs (all messaging channels)
2. **Session Router** â€” Routes to the appropriate agent (multi-agent routing)
3. **Agent Runner** â€” Orchestrates the LLM with Model Resolver (automatic failover between providers)
4. **Tool Executor** â€” Controlled execution (Shell, File System, Browser)
5. **Response Formatter** â€” Formats output for the target channel
6. **Channel Delivery** â€” Delivers to the correct channel

### 2.2 Lane Queue System â€” Key Architectural Innovation

**Philosophy: "Default Serial, Explicit Parallel"**

- **Session Isolation**: Each session has its own "lane"
- **Serial execution by default**: Tasks in a lane execute sequentially â†’ no race conditions, no state corruption
- **Explicit parallelism**: Only idempotent/low-risk tasks (e.g., scheduled background checks) go to parallel lanes
- **Result**: Readable logs, simplified debugging, coherent state, reduced long-term maintenance cost

### 2.3 Semantic Snapshots (Web Navigation)

Instead of screenshots (expensive in tokens and imprecise), OpenClaw parses the **Accessibility Tree (ARIA)**:
- Converts the page to a structured text tree: `button "Sign In" [ref=1]`
- **~90% reduction in token costs** compared to screenshots
- Increased precision for web interaction

### 2.4 Memory System

Hybrid and portable architecture:
- **JSONL Transcripts**: Line-by-line factual audit (messages, tool calls, results)
- **Markdown Memory (MEMORY.md)**: Summaries, experiences, distilled knowledge
- **Hybrid Search**: Vector Search (broad semantic recall) + SQLite FTS5 (keyword precision)
- **Smart Syncing**: Writing to memory â†’ immediate automatic indexing
- **Voyage AI Memory** (new v2026.2.6): Advanced memory backend
- **Real-world case**: A user reported that OpenClaw processed 49,000 facts from chat history to build a knowledge graph, generating proactive reminders and insights

### 2.5 Key Configuration Files

| File | Role |
|------|------|
| `SOUL.md` | Personality, persona, behavioral rules for the agent |
| `AGENTS.md` | Technical instructions, multi-agent security, conventions |
| `USER.md` | Context about the user |
| `MEMORY.md` | Persistent memory |
| `openclaw.json` | Global configuration (5 modules: Gateway, Channels, Skills, Providers, Security) |
| `auth-profiles.json` | Authentication profiles per agent |
| `jobs.json` | Cron job configuration |

**The 5 modules of openclaw.json:**
1. **Gateway**: Port (18789), auth token
2. **Channel**: WhatsApp/Telegram/etc., dmPolicy (pairing/allowlist)
3. **Skills**: 700+ available skills, enable/disable
4. **Provider**: Anthropic, OpenAI, local models, API key config
5. **Security**: Sandbox, audit, exec approvals

### 2.6 Multi-Layer Security

**Execution Security:**
- **Allowlist**: Every command must match a pre-approved pattern (bash, read/write)
- **Structural blocking**: Redirections (`>`), command substitution (`$(...)`), sub-shells (`(...)`), chained execution (`&&`, `||`)
- **Exec Approvals**: Confirmations required for elevated commands
- **Docker sandboxing** recommended for non-main sessions

**Model Resolver / Failover:**
- If a model or key is rate-limited â†’ automatic cooldown â†’ switch to backup
- Multi-provider support: Anthropic, OpenAI, xAI (Grok), Google Gemini, MiniMax, local models (Ollama)
- Multi-provider flights for resilience

---

## 3. Multi-Agent Routing & Orchestration

### 3.1 Core Concept

One agent = one completely isolated "brain":
- Its own **workspace** (files, SOUL.md, skills)
- Its own **state directory** (agentDir) for auth, config
- Its own **session store** (`~/.openclaw/agents/<agentId>/sessions`)
- Its own **auth profiles** (never shared automatically)

### 3.2 Multi-Agent Configuration

```json
{
  "agents": {
    "list": [
      { "id": "home", "default": true, "workspace": "~/.openclaw/workspace-home" },
      { "id": "work", "workspace": "~/.openclaw/workspace-work" },
      { "id": "sandbox", "workspace": "~/.openclaw/workspace-sandbox" }
    ]
  },
  "bindings": [
    { "agentId": "home", "match": { "channel": "whatsapp", "accountId": "personal" } },
    { "agentId": "work", "match": { "channel": "whatsapp", "accountId": "biz" } },
    { "agentId": "sandbox", "match": { "channel": "telegram" } }
  ]
}
```

### 3.3 Routing Rules

- **Most-specific wins**: Deterministic bindings, most specific takes precedence
- **Strict isolation**: NEVER reuse an agentDir between agents (auth/session collisions)
- **Skills per-agent**: Via `<workspace>/skills/`, shared via `~/.openclaw/skills`
- **DM split**: Different people on the same WhatsApp number â†’ different agents (match on sender E.164)
- **Vocabulary**: agentId (brain) â‰  accountId (channel account) â‰  binding (routing rule)

### 3.4 Orchestration Tools

- **`sessions_list`** and **`sessions_send`**: Facilitate inter-session communication
- **Cron + heartbeats**: Background scheduling, work polling
- **Sub-agents with thinking levels**: Differentiated reflection levels for complex workflows

### 3.5 Documented Multi-Agent Patterns

| Pattern | Description |
|---------|-------------|
| **Work/Personal/Sandbox** | Isolation by life context â€” filesystem, memory and permissions separated |
| **Main + Specialized Sub-agents** | Main agent delegates to coder/writer/researcher agents |
| **Polyagent** | Agents that evolve and collaborate, recent focus of @steipete |
| **Per-sender routing** | Each interlocutor is routed to a dedicated agent |
| **Multi-account** | Multiple WhatsApp numbers/Telegram accounts on a single Gateway |

### 3.6 OpenclawInterSystem (OIS)

**Repo:** github.com/Mayuqi-crypto/OpenclawInterSystem

Lightweight framework for communication between OpenClaw instances on **different machines**:
- Communication via `/tools/invoke` HTTP endpoint
- Shared resources: documents, logs, chat history
- Maintained by OpenClaw agents themselves ("CloudMaids")
- MIT License

---

## 4. Skills, Plugins & Ecosystem

### 4.1 Skill Architecture

Each skill = a folder with a `SKILL.md` (YAML frontmatter + natural language instructions). Follows the **AgentSkills** standard — now a **cross-platform standard** shared by Anthropic, OpenAI, and OpenClaw (Feb 2026 convergence). A skill built for one platform can theoretically be moved to any other that adopts the specification.

**Precedence hierarchy:**
1. `<workspace>/skills` (highest priority â€” per-agent)
2. `~/.openclaw/skills` (managed/local â€” shared between agents)
3. Bundled skills (50+ included)
4. `skills.load.extraDirs` in config (lowest priority)

**Plugins:** Can embed their own skills via `openclaw.plugin.json`, participate in normal precedence rules.

### 4.2 ClawHub â€” The Public Registry

- **URL**: clawhub.ai (skills) / onlycrabs.ai (SOUL.md registry)
- **5,705+ skills** as of Feb 7, 2026 (2,999 curated in awesome-openclaw-skills)
- Vector embedding search (text-embedding-3-small)
- npm-style versioning with changelogs + tags
- **VirusTotal partnership** for security scanning
- Stars, comments, curation by admins/mods

```bash
clawhub install <skill-name>
clawhub sync --all
clawhub update <skill-name>
```

### 4.3 Skill Catalog by Category

| Category | Notable Skills | Description |
|----------|---------------|-------------|
| **Productivity** | Todoist, Obsidian Daily, Notion, Gmail | Task management, notes, pages, emails |
| **Health & Home** | WHOOP, Home Assistant, Philips Hue | Health metrics, home automation, lighting |
| **Navigation & Search** | Browser Use, Exa, Tavily Web Search | Autonomous navigation, neural search |
| **Development** | Coding Agent (Codex CLI), GitHub (gh), Clean Code | Code review, deployment, standards |
| **Workflow** | Lobster, RAGLite | Composable pipelines, local RAG |
| **Media** | ElevenLabs TTS, Spotify, FFmpeg Video Editor | Voice, music, video editing |
| **AI & Generation** | fal.ai, Gamma, find-stl | AI images/video, presentations, 3D models |
| **Cloud** | Azure CLI, Azure AI Agents, Coolify | Cloud management, self-hosted PaaS |
| **Monitoring** | News Aggregator (HN, GitHub Trending, PH) | Multi-source tech watch |
| **Social** | Twitter/X, Moltbook | Publishing, presence management |

### 4.4 Creating Custom Skills

```bash
cd ~/.openclaw/skills && mkdir my-skill && cd my-skill
touch SKILL.md
```

**SKILL.md structure:**
```yaml
---
name: my-skill
description: What the skill does
metadata:
  openclaw:
    requires:
      config: ["MY_API_KEY"]
---

# Instructions for the agent
(Natural language instructions on how to use the skill, examples, constraints)
```

**Activation in openclaw.json:**
```json
{
  "skills": {
    "entries": {
      "my-skill": {
        "enabled": true,
        "env": { "MY_API_KEY": "xxx" }
      }
    }
  }
}
```

**Skill creation best practices:**
- One skill = one well-defined responsibility
- Clearly document inputs/outputs with examples
- Check if combined bundled skills already solve the need
- Skills enable autonomous workflows over days/weeks thanks to persistent memory
- Version and allow rollback
- Treat third-party skills as executable code â€” always audit before installation

---

## 5. Complementary Modules & Community Projects

### 5.1 ðŸœ Antfarm â€” Deterministic Multi-Agent Teams

**Author:** Ryan Carson (@ryancarson) â€” creator of ai-dev-tasks (7,500 stars) and Ralph pattern (9,800 stars, 1.8M views on X)
**Repo:** github.com/snarktank/antfarm

```bash
install github.com/snarktank/antfarm
```
â†’ One command provisions everything: agent workspaces, cron polling, subagent permissions.

**3 included workflows:**

| Workflow | Agents | Pipeline |
|----------|--------|----------|
| **feature-dev** | 7 | plan â†’ setup â†’ implement â†’ verify â†’ test â†’ PR â†’ review |
| **security-audit** | 7 | scan â†’ prioritize â†’ setup â†’ fix â†’ verify â†’ test â†’ PR |
| **bug-fix** | 6 | triage â†’ investigate â†’ setup â†’ fix â†’ verify â†’ PR |

**Engineering principles:**
- **Deterministic YAML workflows**: Same steps, same order, every time
- **Cross-verification**: An agent never validates its own work
- **Ralph Loop pattern**: Each agent in fresh session, memory via git + progress files
- **Retry + escalation**: Failures automatically retried, human escalation if exhausted
- **Zero external infra**: YAML + SQLite + cron
- **Dashboard**: `antfarm dashboard`
- **Security**: Only workflows from official repo, prompt injection audit on every PR
- **Extensible**: Create custom workflows in YAML + Markdown

### 5.2 ðŸŒ VoxYZ Agent World â€” Autonomous Closed Loop

**Concept:** 6 AI agents operating a website autonomously (voxyz.space)
**Stack:** OpenClaw (VPS) + Next.js/Vercel + Supabase

**6 roles:**

| Agent | Responsibility |
|-------|---------------|
| Minion | Decisions |
| Sage | Strategic analysis + consolidated memory |
| Scout | Intel collection |
| Quill | Content writing |
| Xalt | Social media management |
| Observer | Quality control |

**3-layer architecture:**

| Layer | Responsibility | Technology |
|-------|---------------|------------|
| Think + Execute | Brain + hands | OpenClaw on VPS |
| Approve + Monitor | Control plane | Vercel |
| Shared State | Single source of truth | Supabase |

**Closed loop:**
```
Proposal â†’ Auto-Approve â†’ Mission + Steps â†’ Worker Execute â†’ Event â†’ Trigger/Reaction â†’ loop
```

**3 critical pitfalls and solutions:**

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Race condition | VPS + Vercel both claimed the same tasks | Single executor (VPS), Vercel = lightweight control |
| Orphaned proposals | Triggers created proposals never converted | Single function `createProposalAndMaybeAutoApprove()` |
| Infinite queue | Approvals despite full quotas | **Cap Gates** â€” rejection at entry |

**Reaction Matrix:**
```json
{ "source": "twitter-alt", "tags": ["tweet","posted"], "target": "growth",
  "type": "analyze", "probability": 0.3, "cooldown": 120 }
```
- `probability < 1.0` â†’ realistic team behavior (not 100% robotic)
- `cooldown` â†’ prevents chain reaction loops
- `recoverStaleSteps` â†’ self-healing for tasks stuck > 30 min

**Timeline:** ~1 week for core loop (excluding pre-existing infra)

### 5.3 Lobster â€” Workflow Shell

Native workflow shell for OpenClaw: typed macro engine, local-first, transforms skills/tools into composable pipelines and safe automations. Allows the agent to call entire workflows in a single command.

### 5.4 Other Ecosystem Projects

| Project | Description |
|---------|-------------|
| **Barnacle** | Utility bot that "attaches" around OpenClaw |
| **Flawd-bot** | Clawd's "evil twin" (flaw detection?) |
| **Clawgo** | OpenClaw node in Go |
| **Clawtinators** | Declarative infra + NixOS modules for hosting |
| **Voice Community** | Community around voice/TTS capabilities |
| **Moltbook** | Social network for AI agents (37,000 registered agents, 1M+ human observers) |
| **Moltworker** | Serverless deployment via Cloudflare Workers |
| **Clawdis** | Nix package for distribution |
| **ClawPhone** | OpenClaw on $25 Android smartphone â€” mobile form factor for agents. Termux + tmux, hardware access (light, sensors, IoT). Also tested on Galaxy Tab S8 |
| **BotDrop** | Dedicated Android APK for OpenClaw â€” config-free install, simplified onboarding. Optional root for system-level access |
| **VisionClaw** | Ray-Ban Meta smart glasses + OpenClaw = JARVIS-style wearable agent. Gemini Live (voice/vision) + 56 OpenClaw tools. Author: @sseanliu (Feb 7, 2026) |
| **openclaw-shield** (Knostic) | Defense-in-depth security plugin: 5 layers (Prompt Guard, Output Scanner, Tool Blocker, Input Audit, Security Gate) |
| **SHIELD.md** (MoltThreat) | Runtime security policy standard for agents, based on structured threat feed |
| **Monty** (Pydantic) | Rust Python interpreter for agents â€” Docker sandboxing alternative, startup <1Î¼s |
| **MEESEEKS** (@doanythingapp) | Auto-cloning agent framework: spawns and manages its own clones (50+ simultaneous agents) |
| **ClawRouter** (BlockRunAI) | Intelligent routing between 30+ models â€” evaluates 14 prompt dimensions in <1ms, routes to cheapest capable model. 400 stars in 48h. USDC payment (Base). github.com/BlockRunAI/ClawRouter |
| **Claw Compactor** | 5-layer deterministic workspace compression. Transcripts 97% smaller, 50%+ global savings, zero LLM cost. Author: @Nielsen777Brian |
| **Spark** | 166 agents with persistent cross-session collective knowledge. Noise filtering + self-correcting loops. Author: @Spark_coded |
| **fairscale-solana** | On-chain reputation skill on Solana â€” real-time wallet verification, human/agent scoring, trust-gated transactions. Author: @fairscalexyz |
| **Nex.ai** | Curated agent templates deployable in <1 min â€” open source, simplified onboarding for non-experts |
| **ClawSec** (Prompt Security) | Complete security suite for OpenClaw agents: SOUL.md drift detection, auto audits, skill integrity, CVE alerts. Open source. github.com/prompt-security/clawsec |
| **clawchain.ai** | On-chain social network for AI agents (Moltbook alternative). Identity, reputation and activity anchored on blockchain (Chromia L1). Wallets + DeFi integrated. Co-founded by Or Perelman (Chromia) |
| **Team9.ai** | AI OS for teams â€” multi-user OpenClaw deployment without friction, centralized auth/context/permissions management. Open source. Founded by Winrey after deploying 50 teammates |
| **BLACKBOX AI Remote Agent** | cloud.blackbox.ai â€” dispatches dev tasks to autonomous agents (clone repo, implement, open PR). Multi-agent execution + chairman LLM. OpenAI SDK compatible |
| **Skilld** | Generates AI agent skills from npm dependencies. Versioned best practices, surfaces API changes, local-first. Author: @harlan-zw. github.com/harlan-zw/skilld |
| **SkillRadar** | Agent-driven skill discovery — tell agent your problem, it searches/compares/recommends skills. v1.1.0 with best practice guide |

---

## 6. Deployment â€” Practical Guide

### 6.1 Deployment Matrix

| Method | For Whom | Pros | Cons |
|--------|----------|------|------|
| **Local (laptop)** | Experimentation | Free, total control | No 24/7 uptime |
| **Mac Mini (M4)** | macOS power users | Native iMessage, Peekaboo, performant | Hardware cost |
| **Old MacBook** | Budget-friendly | Reuse existing hardware | Limited performance |
| **Raspberry Pi** | DIY, always-on | Ultra low-cost, silent | Limited CPU performance |
| **VPS (see comparison below)** | Production | Persistent, accessible anywhere | Server management |
| **Docker** | Maximum security | Sandboxing, rollback, isolation | Docker complexity |
| **DO App Platform** | Scalable multi-agents | Managed, elastic scaling | Platform cost |
| **DO 1-Click** | Quick start | Pre-configured security | Less control |
| **Cloudflare Workers** | Serverless | No server to manage | Worker limitations |
| **Fly.io / Render** | Easy scaling | Simple deployment | Variable costs |
| **Oracle Cloud** | Free tier | Free (free tier) | Oracle complexity |
| **NixOS** | Reproducibility | Declarative, deterministic | Learning curve |
| **Smartphone ($25)** | Ultra-mobile | ClawPhone â€” physical mobile agent, hardware access | Experimental, limited perf |
| **EasyClaw / SimpleClaw** | Non-technical users | No terminal, 1-2 min deploy | Limited customization |
| **Claw Cloud** | Agent-optimized infra | Auto-scaling, state management, health monitoring | New platform |
| **Railway / Render** | Auto-scaling PaaS | Git push deploy, pay-as-you-go | Variable costs |
| **MissionControlAI** | Multi-agent orchestration | Web UI, monitoring, coordination | $50+/month |

### 6.1bis VPS Comparison for OpenClaw (Feb 2026 data)

| Provider | Typical Config | Price/month | Setup Ease | 1-click OC | Verdict |
|----------|---------------|-------------|------------|------------|---------|
| **Hetzner** | 2 vCPU, 4GB, 40GB NVMe | â‚¬3.49-4.09 | â­â­â­ | âŒ | ðŸ’° **Best budget** â€” unbeatable price/perf, GDPR, 20TB transfer |
| **Hostinger** | 2 vCPU, 4GB NVMe | ~$5-7 | â­â­â­â­â­ | âœ… | ðŸ† **Best overall** â€” AI assistant Kodee (MCP), DDoS protection, Docker GUI |
| **DigitalOcean** | 2 vCPU, 4GB | $24 | â­â­â­â­â­ | âœ… | ðŸš€ **Easiest** â€” best docs, $200 trial credits, ideal for beginners |
| **AWS Lightsail** | 2 vCPU, 4GB | $24 | â­â­â­â­ | âŒ | â˜ï¸ AWS ecosystem â€” predictable pricing, easy EC2 migration |
| **Linode/Akamai** | 2 vCPU, 4GB | $24 | â­â­â­ | âŒ | Reliable, Akamai-backed, but no marketplace |
| **Cloudflare** | Workers/Containers | $5 base + usage | â­â­ | âŒ | âš ï¸ **Not for VPS** â€” excellent as front/gateway (free AI Gateway) |

**Community Recommendation:** Hetzner for technical users, Hostinger for less friction, DigitalOcean for first steps. **Tip:** Add Cloudflare AI Gateway in front for free regardless of chosen provider.

*Source: community comparison by @delphi_labs, February 9, 2026*

### 6.1ter Deployment Ecosystem — 5-Tier Framework (Feb 2026)

*Source: "25 Ways to Deploy Your OpenClaw Agent" — ZHC Research, February 11, 2026*

The deployment landscape has exploded to **25+ distinct platforms** across 5 tiers:

| Tier | Platforms | Cost | For Whom |
|------|-----------|------|----------|
| **1. One-Click Deployers** | EasyClaw, SimpleClaw, StartClaw (~$10/mo), QuickClaw (iOS), ClawDuck (multi-LLM), Spin My Claw (white-label) | $0-30/mo | Non-technical founders, fastest time-to-deploy |
| **2. VPS/Cloud** | Oracle Cloud (free tier: 4 CPU, 24GB RAM), Hetzner, Hostinger, DigitalOcean, AWS EC2, **Claw Cloud** (agent-optimized) | $0-50/mo | Technical builders wanting control |
| **3. Serverless/PaaS** | Railway, Render, Cloudflare Moltworker (edge, 200+ locations) | $5-25/mo | Auto-scaling, variable traffic |
| **4. Multi-Agent Orchestration** | MissionControlAI ($50+), OpenClaw Mission Control (self-hosted, free) | $0-200/mo | 10+ agents in production |
| **5. DIY / Custom** | NixOS, ClawPhone, Raspberry Pi, custom Docker | $0+ | Full control, edge deployments |

**Key new platforms identified:**

- **Oracle Cloud Free Tier**: 4 ARM CPUs, 24GB RAM, 200GB storage — $0/mo permanently. Most generous free tier available. ZHC uses it internally for dev environments.
- **Claw Cloud** (clawcloud.co): Purpose-built for agents — auto-scaling based on agent activity, health monitoring, agent-to-agent networking, automatic memory/state backup. Free tier available.
- **EasyClaw**: No terminal, 2-min deploy via web UI or Telegram. Free tier.
- **MissionControlAI**: Web UI for fleet management, coordinating 10+ agents, shared resources.
- **ClawDuck**: OpenRouter integration for multi-LLM switching. $2 credit top-ups for pay-as-you-go.

**Stage-based selection (ZHC recommendation):**

| Stage | Monthly Cost | Recommended | Upgrade When |
|-------|-------------|-------------|--------------|
| Testing | $0 | Oracle Cloud or ClawDuck | Need 24/7 uptime |
| Launch | $3-10 | Hetzner or EasyClaw | 100+ daily users |
| Scale | $20-50 | Railway or Render | Traffic unpredictable |
| Multi-Agent | $50-200 | MissionControlAI or self-hosted | Coordinating 5+ agents |

### 6.2 Secure Docker Configuration (Recommended)

```yaml
version: '3.8'
services:
  openclaw:
    image: openclaw/agent:latest
    security_opt:
      - no-new-privileges:true
    read_only: true
    user: "1000:1000"
    cap_drop:
      - ALL
    tmpfs:
      - /tmp:rw,noexec,nosuid,size=64M
    volumes:
      - ./config:/home/openclaw/.openclaw:ro
      - ./state:/home/openclaw/state:rw
      - ./workspace:/workspace:rw
    ports:
      - "127.0.0.1:18789:18789"
    restart: unless-stopped
```

**Absolute Docker rules:**
- âŒ NEVER mount the complete home directory
- âŒ NEVER mount the Docker socket
- âŒ NEVER expose port 18789 publicly
- âœ… Read-only filesystem + targeted volumes
- âœ… Drop ALL capabilities
- âœ… Non-root user (1000:1000)
- âœ… Share base image between agents (disk savings: 5 agents = +1-2 GB)

### 6.3 Installation & Onboarding

```bash
# Verify official package
npm view openclaw dist-tags

# Install a specific patched version
npm install -g openclaw@2026.2.6

# Verification
openclaw --version

# Onboarding wizard (RECOMMENDED)
openclaw onboard

# Diagnostics
openclaw doctor
```

### 6.4 Essential Security Hardening

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

# Audit
openclaw security audit
```

### 6.5 Secure Network Exposure

**NEVER expose port 18789 directly to the Internet.**

| Method | Description |
|--------|-------------|
| **Tailscale Serve/Funnel** | Private network, `openclaw.your-tailnet.ts.net` |
| **SSH tunnels** | Secure access without exposure |
| **VPN** | Restricted access to web dashboard |
| **IP Allowlist + Firewall** | Restrictive by default |
| **Headless (CLI only)** | No web UI, access via `doctl apps console` |

### 6.6 Pragmatic Hardening (IZHC Discord Field Reports)

The community reveals a recurring dilemma: **strict security vs operational friction**. Creating separate Linux users with granular permissions consumes too much time for a personal setup. Here's the pragmatic consensus:

**Pointbreak Pattern (recommended for dedicated agent VPS):**
1. **Store nothing sensitive on the VPS** â€” treat the machine as disposable
2. **Human-in-the-loop for critical actions** â€” crypto transactions = multisig, deployments = approval
3. **Frequent automatic backups** (DO/Hetzner snapshots 2x/day) â€” if compromised, fast rollback
4. **Isolate credentials** â€” API keys with minimal scopes, dedicated tokens, budget caps
5. **Accept residual risk** â€” an LLM is a statistical model, it will do unexpected things

**Anti-pattern "yolo mode":** Giving sudo to the agent on a dedicated VPS is tempting ("just let it run riot") but exposes you to total destruction via prompt injection. The compromise: broad filesystem access WITHOUT sudo, with backups.

**Documented pitfall:** A user burned a $150 budget cap in a single Opus 4.5 planning session â€” cost isn't always visible in real-time. Always configure spend threshold alerts in addition to budget caps.

---

## 7. Security â€” Critical Points

### 7.1 CVE-2026-25253 (CVSS 8.8 â€” Fixed v2026.1.29)

**Vulnerability:** Web interface accepted `gatewayUrl` via query strings â†’ WebSocket without origin header validation
**Impact:** Malicious link â†’ token theft â†’ complete RCE in milliseconds â€” even on localhost
**Fix:** v2026.1.29+, rotate tokens, never include token in URL

### 7.2 Documented Threats

| Threat | Details |
|--------|---------|
| **Network prompt injection** | Compromised agent â†’ can compromise others via normal interactions (Moltbook) |
| **Malicious skills** | 230+ malicious skills identified on ClawHub stealing keys/wallets |
| **ClawHavoc** | Malware campaign targeting OpenClaw deployments |
| **SSRF / Excessive Agency** | 80% of tests show SSRF risks and excessive agency |
| **Fake sites** | Fraudulent sites pretending to distribute OpenClaw (documented by Forbes) |
| **Uncontrolled API costs** | $20 of API burned while sleeping (documented case) |
| **Inter-session leaks** | Data crossing sessions without strict isolation |
| **Naive Nginx proxy** | Nginx forward â†’ Gateway sees localhost â†’ auto-approves malicious requests |
| **mDNS info leak** | Gateway broadcasts mDNS exposing cliPath, hostname, SSH â€” facilitated reconnaissance. Fix: minimal mode or `OPENCLAW_DISABLE_BONJOUR=1` |
| **42,665 exposed instances** | Found online by CrowdStrike, 8 without any authentication |
| **135K+ exposed instances (STRIKE)** | SecurityScorecard STRIKE: 135,000+ internet-exposed instances (Feb 9), 50,000+ vulnerable to known RCE, exponential growth |
| **900 malicious skills (Bitdefender)** | ~20% of ClawHub registry is malicious. 14 actors identified. Automated campaigns via compromised GitHub accounts |
| **Indirect prompt injection (Zenity)** | Malicious Google Doc â†’ agent creates attacker Telegram integration â†’ SOUL.md modification + cron for persistence â†’ C2 Sliver escalation. Zero-click |
| **Credential leaks (Snyk)** | 283 skills (7.1%) expose API keys, passwords, wallets in plaintext |
| **Lethal Trifecta (Palo Alto)** | Private data access + untrusted content exposure + external communication = agents vulnerable by design |
| **Default Admin (BitsecAI VULN-188)** | WebSocket handshake without `scopes` field â†’ server grants `operator.admin` (god mode). Trivial exploit |
| **Context Rot (Team9.ai)** | Agent involuntarily leaks sensitive data by hallucinating context boundaries |

### 7.3 Complete Security Checklist

- [ ] Version >= 2026.2.9 (Telegram, Windows path, hooks fixes)
- [ ] Tokens rotated regularly, NEVER in URLs
- [ ] Port 18789 not publicly exposed
- [ ] Docker with read-only filesystem, cap-drop ALL, non-root
- [ ] Skills audited (VirusTotal on ClawHub + source code review)
- [ ] DM policy in allowlist or pairing (NEVER "allow all")
- [ ] Spend limits on model provider
- [ ] Dedicated API key for OpenClaw with max budget
- [ ] `openclaw security audit` run regularly
- [ ] `openclaw doctor` for diagnostics
- [ ] No naive reverse proxy
- [ ] Browser/cron disabled in groups
- [ ] Sandbox environment for experiments
- [ ] SHIELD.md configured with active threat feed
- [ ] openclaw-shield (Knostic) installed in enforce mode
- [ ] mDNS in minimal mode or disabled (`OPENCLAW_DISABLE_BONJOUR=1`)
- [ ] Bitdefender AI Skills Checker used for pre-install scanning
- [ ] ClawSec installed for SOUL.md drift detection + CVE alerts
- [ ] File Integrity Monitoring on SOUL.md, AGENTS.md, MEMORY.md
- [ ] Frontier model (Opus/GPT-4) for tool-enabled agents â€” weaker models more injection-vulnerable

### 7.4 Dedicated Security Tools

| Tool | Author | Description |
|------|--------|-------------|
| **openclaw-shield** | Knostic | 5-layer defense-in-depth plugin: Prompt Guard, Output Scanner, Tool Blocker, Input Audit, Security Gate |
| **SHIELD.md** | MoltThreat | Runtime security policy standard â€” structured threat feed with block/approve/log decisions |
| **Skill Scanner** | Cisco AI Defense | Open-source malicious skill analysis tool |
| **agent-shield** | ultimatebos | Curated community blocklist + Chitin Protocol for reporting |
| **Bitdefender AI Skills Checker** | Bitdefender | Free ClawHub skill security analysis â€” backdoor, exfiltration, prompt injection, obfuscation detection |
| **VirusTotal Code Insight** | Google/VirusTotal | Automated scanning of all ClawHub skills via Gemini â€” official partnership (Feb 2026) |
| **ZeroLeaks** | Community | Prompt injection resistance analysis â€” OpenClaw scores 2/100 |
| **ClawSec** | Prompt Security | Complete security suite: SOUL.md/skill drift detection, automated audits, skill integrity, auto-updated CVE alerts. Open source |

### 7.5 Reference Security Analyses

- **Cisco**: "Personal AI Agents like OpenClaw Are a Security Nightmare"
- **CrowdStrike**: "What Security Teams Need to Know About OpenClaw"
- **Vectra AI**: "From Clawdbot to OpenClaw: When Automation Becomes a Digital Backdoor"
- **Consortium.net**: Complete Security Advisory
- **Knostic**: "Building openclaw-shield: Lessons Learned Securing OpenClaw Agents"
- **Wiz**: Critical investigation of Moltbook
- **Auth0**: "Five Step Guide Securing OpenClaw"
- **SecurityScorecard STRIKE**: 135K+ exposed instances, live threat dashboard
- **Bitdefender**: "Technical Advisory: OpenClaw Exploitation in Enterprise Networks"
- **BitsecAI**: First holistic codebase audit (400K+ LOC, 100+ vulnerabilities)
- **Palo Alto Unit 42**: "Lethal Trifecta" concept
- **Snyk**: "Inside the ClawHub Malicious Campaign"
- **Kaspersky**: "OpenClaw AI agent found unsafe for use" — mainstream vendor synthesis of risks + home user recommendations (Feb 10, 2026)
- **SOCRadar**: CVE-2026-25253 deep dive — links to Patch Tuesday, broader AI framework risks (Feb 11, 2026)
- **Cyera Research Labs**: "The OpenClaw Security Saga" — data governance analysis, introduces "Data Gravity" concept: agent aggregating OAuth/API/SaaS permissions creates disproportionate compromise reach (Feb 2026)
- **Dark Reading**: "OpenClaw AI Runs Wild in Business Environments" — Token Security analysis of non-human identity risks

---

## 8. Cost Optimization & Model Selection

### 8.1 The Cost Problem

OpenClaw sends the **complete context** with every request:
- 120,000 tokens of context = ~$1.80 input/message with Opus
- Context window bloat is the primary cost driver
- Without control, an active agent can burn tens of $/day

### 8.2 Tiering Strategy (Community Best Practice)

| Task | Model | Reason |
|------|-------|--------|
| Complex reasoning, deep research | Claude Opus / GPT-4 | Maximum quality |
| Routine tasks, conversation | Claude Sonnet / Grok | Good quality/cost ratio |
| Templated tasks, summaries, extraction | Local models (Ollama) / Haiku | Near-zero cost |
| Real-time interactions, support | Compact local model | Minimal latency |
| Fresh sessions (Ralph pattern) | Sonnet/Haiku | Minimal context = low costs |

**Supported models:** Anthropic (Claude), OpenAI (GPT), xAI (Grok — new v2026.2.6), Google Gemini, MiniMax (m2.1 community-recommended), Moonshot (Kimi-K2.5 — **#1 model on OpenClaw via OpenRouter**), Z.ai GLM-5 (744B MoE, MIT license, SWE-bench 77.8% — new Feb 11), GLM 4.7 (355B MoE, 200K context, free via Ollama), local models via Ollama

**Official steipete recommendation (README Feb 2026):** "I strongly recommend Anthropic Pro/Max (100/200) + Opus 4.6 for long-context strength and better prompt-injection resistance."

**âš ï¸ Opus 4.6 community alert (Discord IZHC, Feb 2026):** Opus 4.6 consumes significantly more tokens than Opus 4.5 for the same tasks. Multiple users report "insane" consumption making the model "basically unusable" for planning. Context injection (skills + prompts + memory injected before execution) amplifies the problem. **Buz's advice (IZHC):** "Sense check just how much is going into your context for planning."

### 8.3 Cost Reduction Tips

- **Memory compaction**: OpenClaw automatically compresses long sessions
- **`max_tokens_per_request: 4000`**: Limit for routine tasks
- **Local model for heavy lifting**: Ollama + quantized model
- **Dedicated API key with budget cap**
- **Ralph Pattern**: Fresh sessions = no context accumulation
- **Token dashboard** (new v2026.2.6): Consumption visualization
- **Hybrid deployment**: Local models for real-time, hosted models for complex reasoning

### 8.4 Community Cost Optimization Tools

| Tool | Author | Description | Claimed Savings | Caution |
|------|--------|-------------|-----------------|---------|
| **ClawRouter** | @bc1beat / BlockRunAI | Intelligent routing between 30+ models. Evaluates 14 prompt dimensions in <1ms. | ~70% | **Payment via USDC wallet (Base)** â€” proxy intermediary, supply-chain risk |
| **Claw Compactor** | @Nielsen777Brian | 5-layer deterministic workspace compression. Zero LLM cost. | 50%+ workspace, 97% transcript reduction | "Mostly lossless" â€” evaluate info loss. Complementary to native v2026.2.9 compaction |

**Community Recommendation:** Prefer manual tiering (Â§8.2) or Claw Compactor (deterministic, no intermediary) over ClawRouter until the crypto-wallet model is audited.

---

## 9. Success Stories & Use Cases

### 9.1 Personal Productivity & Daily Life

| Use Case | Details |
|----------|---------|
| **Home server with 15 automated jobs** | WHOOP health monitoring, calendar, personalized briefings, 49,000 facts processed |
| **"Self-healing infrastructure"** | Agent monitors system health, fixes problems, writes blog posts about its operations |
| **Morning summary** | Email + calendar + tasks aggregated into concise summary |
| **Automated shopping** | Price monitoring, purchase when threshold reached |
| **Family assistant** | Hue, Sonos, purifier control + traffic/schedule-based reminders |

### 9.2 Development & DevOps

| Use Case | Details |
|----------|---------|
| **CI/CD monitoring** | Failure notifications, deployment management |
| **Auto code review + tests** | Automatic tests, Sentry error capture, resolution + PRs |
| **Transfer documents** | Notion + GitHub analysis â†’ personalized transfer documents in minutes |
| **Vibe coding** | Clicker game coded, GitHub commit, Vercel auto-deploy without human review |
| **OAuth auto-config** | Agent opens browser, accesses Google Cloud Console, configures OAuth |

### 9.3 Business & Enterprise

| Use Case | Details |
|----------|---------|
| **Recruitment** | Automated LinkedIn sourcing + candidate emails |
| **Content pipeline** | Design, code review, PM, taxes, content â€” "AI as teammate, not tool" |
| **VoxYZ Agent World** | 6 agents (Minion/Sage/Scout/Quill/Xalt/Observer) operating autonomous website. Full RPG character system: 6-layer Role Cards, Affinity Matrix (15 pairwise relationships with drift), memory-driven personality evolution, gamified RPG stats. Stack: Hetzner+Supabase+Vercel ~$25/mo. → TD Annex E.8-E.11 |
| **Stripe Minions** | 1000+ PRs/week unattended, Goose fork, Toolshed MCP (400+ tools), isolated devboxes in 10s |
| **Goldman Sachs + Anthropic** | Autonomous accounting/vetting agents as "digital coworkers" |
| **Mac Mini Fleets (China)** | Mac Mini racks hosting OpenClaw agents as "24/7 employees" |
| **8 agents content marketing (ScreenSnap Pro)** | Pipeline: Scoutâ†’Quillâ†’Sageâ†’Ezraâ†’Herald + PM agent Morgan. 80+ articles in 10 days, $0.70/article, 15min/day human time. Claim locking, quality gates (40% first-draft rejection) |
| **50 teammates (Team9.ai / Winrey)** | Team-scale deployment by ADHD founder. Key learnings: "One-Click Install guides are a lie" → built cloud-native auto-deploy ("LEAVE ME ALONE!!!"). Identified 3 systemic problems: Personal Plugin Problem (non-shareable workflows), Integration Tax (OAuth gauntlet per user), Context Rot (agent leaks floor price to partner by hallucinating context boundary). Solution: Team9.ai as "AI OS for teams" — infinite agents, zero local setup, role-based context isolation. Now open-source. 10x velocity post-migration |

### 9.4 Notable Community Quotes

> "After years of AI hype, I thought nothing could faze me. Then I installed OpenClaw. AI as teammate, not tool." â€” @lycfyi

> "Processed our entire source of truth via WhatsApp in minutes, where RAG agents struggled for days." â€” @pocarles

> "It will actually be the thing that nukes a ton of startups, not ChatGPT. The fact that it's hackable and self-hackable..." â€” @rovensky

> "It's far from perfect â€” but the system genuinely runs, genuinely doesn't need someone watching it." â€” VoxYZ author

---

## 10. Limitations, Critiques & Emerging Concepts

### 10.1 Security

- CVE-2026-25253 (one-click RCE via WebSocket) â€” fixed but exposed thousands of instances
- 230+ malicious skills on ClawHub stealing keys/wallets
- ClawHavoc malware campaign
- SSRF and excessive agency in 80% of security tests
- Plaintext credential storage
- 42,665 exposed instances found online
- Qualified as "security dumpster fire" by some analysts
- Network-scale prompt injection via Moltbook

### 10.2 Technical Limitations

- **High token usage / inefficiency**: Complete context sent with every request
- **Not suited for sensitive data** without strict sandboxing
- **Insecure defaults**: Many dangerous parameters enabled by default
- **Complex setup** for non-technical users
- **Inter-session data leaks** without strict isolation
- **Browser relay instability** in some configurations
- **iMessage restart loops** (known bug)
- **Config collisions** in multi-agent if agentDir is shared

### 10.3 Architectural Limitations

- **Not enterprise-ready**: Missing RBAC, SSO, audit logging, SLA guarantees
- **No true native collaborative multi-agent**: One agent does many things, but inter-agent coordination is still basic/hacky
- **Physical actions impossible**: No robotic arm (can order online, can't make coffee)
- **LLM dependency**: If no skill or prompt exists for a task, the agent fails
- **Shadow AI risk** in enterprise: Uncontrolled adoption by employees
- **Context Rot**: Agent hallucinating context boundaries â†’ sensitive data leak cross-session (Team9.ai)
- **Personal Plugin Problem**: Workflows = non-shareable "private cheat codes" (Team9.ai)
- **Integration Tax**: Per-user auth (OAuth scopes, tokens, expiration) consumes more time than it saves at team scale
- **"One-Click Install" myth**: Every local environment is a unique snowflake (confirmed at 50 users)
- **Multi-agent race conditions**: Parallel agents on shared state require claim locking

### 10.4 Emerging Ecosystem Concepts

**Living Files vs Dead Files** (community theory, Feb 2026)

Fundamental distinction:
- **Dead file**: Inert document â€” does nothing without manual intervention
- **Living file**: Markdown on VPS accessible 24/7 by an AI agent that reads it, updates it, references it, and acts on it

Key implication: "If you cannot verbalize it, you cannot automate it." The ability to externalize processes, preferences, and knowledge into structured markdown files is the differentiating skill. Each added file compounds the system's value.

**Multi-agent dispatch + chairman pattern** (BLACKBOX AI)

Emerging pattern: dispatch the same task to multiple agents simultaneously, then a "chairman LLM" evaluates all implementations and selects the best one. Applicable beyond code â€” any task where comparing approaches improves quality.

**Feedback loops & self-improving agents** (Claude Code /insights)

Pattern: automatically analyze your own usage patterns to identify friction and generate fixes. 3 universal frictions identified (transferable to OpenClaw):
1. **Ambiguous references** â†’ agent sprints toward wrong context. Fix: 3 lines in SOUL.md specifying key directories/projects
2. **"Mostly done" pattern** â†’ 91% sessions incomplete because "done" was never explicitly defined. Fix: acceptance criteria per task
3. **Exploration overhead** â†’ agent explores too much before acting. Fix: more upfront context on structure

**Research â†’ Skills â†’ Build** (Vibe Marketing)

3-layer framework: (1) deep research via MCP, (2) build specialized stackable skills, (3) produce assets. "Skill stacking" â€” one skill feeds the next in a pipeline â€” is a reusable architectural pattern.

**"Autopilot Era" — from Copilot to Autonomous Execution** (industry trend, Feb 2026)

Analysts describe enterprise software's shift from reactive SaaS to agentic systems as the "autopilot phase" — where AI agents monitor signals, trigger workflows, and complete tasks autonomously within guardrails. Key implications: SaaS pricing may shift from seats-licensed to outcomes-delivered. Job design evolves from executing tasks to supervising AI outputs, refining prompts, and managing exceptions. Governance, auditability, and explainability become key vendor differentiators. *Source: "Software enters the autopilot era with AI agents" (Feb 11, 2026)*

**"Data Gravity" for Agents** (Cyera Research Labs, Feb 2026)

New security concept: once an agent aggregates OAuth tokens, API keys, and SaaS permissions, it creates a "data gravity well" — any compromise gains disproportionate reach across all connected services. Unlike traditional data breaches (one system), an agent compromise potentially exposes every service the agent touches. Implication: agent identity should be treated as a privileged security principal (per Auth0's "Agent-as-Security-Principal" model).

**Commercial ecosystem emergence** (Feb 2026)

First documented commercial exit: a 1-click OpenClaw deployment service sold for **$225,000** with $17K MRR and 397 subscribers. Signals commercial viability of OpenClaw-adjacent services. The deployment landscape has expanded to 25+ platforms (see §6.1ter), from free tiers to enterprise orchestration — a sign the ecosystem is maturing beyond hobbyist usage.

---

## 11. Navigation to Other Documents

| Document | Content | File |
|----------|---------|------|
| **Project Concept** | Vision, architecture, roadmap, JUNO tokenomics | `OpenClaw_Expert_Agent_Project.md` |
| **Operator's Playbook** | Step-by-step: install, config, security, multi-agents | `OpenClaw_Operators_Playbook.md` |
| **Technical Deep Dives** | In-depth: security, MCP, voice, patterns... | `OpenClaw_Technical_Deep_Dives.md` |
| **Ecosystem Watch** | Releases, articles, chronological signals | `OpenClaw_Ecosystem_Watch.md` |
| **WSL2 Setup Guide** | Step-by-step Windows + WSL2 installation | `OpenClaw_Setup_Guide_WSL2.md` |
| **Knowledge Index** | Thematic cross-reference navigation | `OpenClaw_Knowledge_Index.md` |
| **Contributing Guide** | How to contribute to the corpus | `CONTRIBUTING.md` |

---

*OpenClaw Encyclopedia â€” Community Edition v1.0. Translated and adapted from the LobsterOps knowledge base. Last update: February 10, 2026.*
*Initial research and compilation: LobsterOps project. Open for community contributions via the IZHC.*
