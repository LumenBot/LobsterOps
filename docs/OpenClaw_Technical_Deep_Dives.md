# ðŸ¦€ OpenClaw Technical Deep Dives â€" Community Edition

> **Role:** In-depth technical appendices complementing the Encyclopedia and Playbook.
> Each annex dives deep into a specific topic from the research backlog.
>
> **Version:** 1.1-community | **Last updated:** February 12, 2026
> **Origin:** Translated and adapted from LobsterOps Annexes Techniques v1.3

---

## TABLE OF CONTENTS

- [Annex A: Lobster â€" Workflow Shell](#annex-a)
- [Annex B: MCP Integration (Model Context Protocol)](#annex-b)
- [Annex C: Voice Capabilities â€" TTS, STT, Edge Fallback](#annex-c)
- [Annex D: OpenClaw vs Multi-Agent Frameworks](#annex-d)
- [Annex E: Advanced SOUL.md Patterns â€" Persona Engineering](#annex-e)
- [Annex F: Moltbook â€" The AI Social Ecosystem and Its Risks](#annex-f)
- [Annex G: RAG with OpenClaw â€" RAGLite & Internal Documents](#annex-g)
- [Annex H: Advanced Browser Automation](#annex-h)
- [Annex I: Webhooks & Event-Driven Workflows](#annex-i)
- [Annex J: Memory Compaction â€" Detailed Mechanics](#annex-j)
- [Annex K: Roundtable Voting â€" Multi-Agent Consensus (VoxYZ)](#annex-k)
- [Annex L: Enterprise Use Cases â€" RBAC Workaround Strategies](#annex-l)
- [Annex M: Advanced Security â€" SHIELD.md, openclaw-shield & Tools](#annex-m)
- [Annex N: Monty â€" Containerless Sandboxing for Agents](#annex-n)
- [Annex O: Stripe Minions â€" Enterprise Pattern for Code Agents](#annex-o)
- [Annex P: Collective Intelligence â€" The Next Multi-Agent Bottleneck](#annex-p)
- [Annex Q: Cost Optimization â€" Emerging Community Tools](#annex-q)
- [Annex R: Field Reports â€" Discord IZHC (Feb 5-10, 2026)](#annex-r)
- [Annex S: Multi-Agent Blueprint â€" Content Marketing Pipeline (ScreenSnap Pro)](#annex-s)
- [Annex T: Detailed VPS Comparison & "Living Files" Concept](#annex-t)
- [Annex U: Feedback Loops, Self-Improving Agents & Skill Stacking](#annex-u)
- [Annex V: Battle-Tested Patterns - 24/7 Autonomous Operations](#annex-v)
- [Annex W: SKILL.md Standard Convergence - Cross-Platform Skills](#annex-w)

---

## Annex A: Lobster â€" Workflow Shell {#annex-a}

### A.1 Concept

Lobster is OpenClaw's **native workflow runtime**: a typed macro engine, local-first, that transforms tool sequences into deterministic pipelines with explicit approval gates.

**The problem it solves:** Without Lobster, a complex workflow requires N tool calls between the agent and the LLM. Each call costs tokens, and the LLM must re-orchestrate every step. Lobster moves this orchestration into a deterministic runtime.

**Concrete benefits:**
- **1 call instead of N**: OpenClaw makes a single `lobster.run()` and receives a structured result
- **Massive token savings**: a 10-step workflow goes from 10Ã- (LLM planning overhead) to 1 compact call
- **Approval gates**: side effects (email sending, comment posting) **physically stop** the workflow until explicit approval
- **Resumable**: a stopped workflow returns a resume token; approve and resume without restarting
- **Auditability**: pipelines are data (YAML/JSON) â†' easy to log, diff, replay, review

### A.2 Pipeline Syntax

```json
{
  "action": "run",
  "pipeline": "exec --json --shell 'inbox list --json' | exec --stdin json --shell 'inbox categorize --json' | exec --stdin json --shell 'inbox apply --json' | approve --preview-from-stdin --limit 5 --prompt 'Apply changes?'",
  "timeoutMs": 30000
}
```

### A.3 Design Principles

- **Typed pipelines**: Data flows as objects/arrays (JSON), not raw text
- **Local-first**: Local execution, no cloud dependency
- **No auth surface**: Lobster never handles OAuth/tokens â€" it uses OpenClaw's
- **Built-in safety policy**: Timeouts, output caps, sandbox checks and allowlists enforced by runtime
- **Minimal grammar**: Small language + JSON piping = few "creative" paths, realistic validation

### A.4 Implementation Recommendations

1. Identify repetitive workflows with high token cost
2. Convert them to Lobster pipelines (YAML/JSON)
3. Place approval gates before any irreversible side effect
4. Use state for inter-session resume
5. Combine with cron jobs for scheduled execution

---

## Annex B: MCP Integration (Model Context Protocol) {#annex-b}

### B.1 Current State

**Native MCP is not yet integrated into OpenClaw core.** GitHub issue #4834 was closed "not planned" by maintainers. However, the community has produced several solutions:

### B.2 Community MCP Solutions

| Solution | Approach | Transport | Link |
|----------|----------|-----------|------|
| **openclaw-mcp** (@freema) | MCP â†' Claude.ai bridge via OAuth 2.1 | SSE/HTTP | glama.ai |
| **openclaw-mcp-adapter** | Plugin exposing MCP tools as native OpenClaw tools | stdio/HTTP | github.com/androidStern |
| **openclaw-mcp-plugin** | HTTP Streamable plugin for remote MCP | HTTP/SSE | github.com/lunarpulse |
| **openclaw-claude-code-skill** | Skill for MCP integration + sub-agent orchestration | Multiple | LobeHub |

### B.3 MCP Adapter Configuration (Recommended for Native Use)

```json
{
  "plugins": {
    "entries": {
      "mcp-adapter": {
        "enabled": true,
        "config": {
          "servers": [
            {
              "name": "filesystem",
              "transport": "stdio",
              "command": "npx",
              "args": ["-y", "@anthropic/mcp-filesystem", "/path/to/dir"]
            },
            {
              "name": "tavily",
              "transport": "http",
              "url": "http://localhost:3000/mcp",
              "headers": { "Authorization": "Bearer ${API_TOKEN}" }
            }
          ]
        }
      }
    }
  }
}
```

### B.4 MCP Implementation Recommendations

1. **Use mcp-adapter** to expose MCP servers as native tools (most transparent)
2. For web search: Tavily MCP > Brave API > DuckDuckGo (free)
3. For up-to-date library documentation: Context7 MCP
4. Monitor OpenClaw issue tracker for potential future native support
5. MCP enables connection to Notion, Linear, Stripe, PostgreSQL, Blender, and any community MCP server

---

## Annex C: Voice Capabilities â€" TTS, STT, Edge Fallback {#annex-c}

### C.1 Voice Architecture

OpenClaw supports voice on **macOS, iOS, and Android** natively, and via TTS/STT on all messaging channels.

**Complete voice pipeline:**
```
Incoming audio â†' STT (Whisper) â†' Text â†' LLM â†' Response text â†' TTS â†' Outgoing audio
```

### C.2 Supported TTS Providers

| Provider | API Key Required | Quality | Cost | Latency |
|----------|-----------------|---------|------|---------|
| **OpenAI** (gpt-4o-mini-tts) | Yes | Excellent | Paid | ~1-2s |
| **ElevenLabs** (eleven_multilingual_v2) | Yes | Premium, clonable voices | Paid | ~1-2s |
| **Edge TTS** | No (free, keyless) | Good | Free | Variable |

**Fallback cascade:** OpenAI â†' ElevenLabs â†' Edge TTS (if no API key configured)

### C.3 TTS Commands

```
/tts always       # Enable TTS for all responses
/tts off          # Disable
/tts inbound      # TTS only when user sends a voice message
/tts provider openai  # Change provider
/tts status       # Show current config
```

### C.4 Implementation Recommendations

1. **Start with Edge TTS** (free, no key needed) for testing
2. Switch to **OpenAI TTS** for production quality
3. **ElevenLabs** for voice cloning or premium multilingual voices
4. Use `/tts inbound` to trigger voice only on incoming voice messages
5. On **Telegram**: voice notes workflow is the smoothest
6. Configure `summaryModel` to auto-summarize long responses before TTS

---

## Annex D: OpenClaw vs Multi-Agent Frameworks {#annex-d}

### D.1 Positioning

OpenClaw is **not** a multi-agent framework in the same sense as CrewAI, LangGraph, or AutoGen. It's a **personal agent runtime** with added multi-agent capabilities. The relevant comparison is on **approach** rather than raw features.

### D.2 Comparison Matrix

| Criteria | OpenClaw | CrewAI | LangGraph | AutoGen |
|----------|----------|--------|-----------|---------|
| **Philosophy** | Self-hosted personal agent | Role-based teams | State graphs | Multi-agent conversations |
| **Native multi-agent** | Via routing + bindings | Yes (Crews) | Yes (Nodes/Edges) | Yes (Conversations) |
| **Orchestration** | Cron + heartbeats + Lobster | Process/Task-based | Graph DAG | Event-driven async |
| **Memory** | JSONL + MEMORY.md + Vector/FTS5 | ChromaDB + SQLite | In-thread + cross-thread | Context variables |
| **Messaging channels** | 15+ native channels | No (API only) | No (API only) | No (API only) |
| **Self-hosted** | Yes (core) | Yes | Yes | Yes |
| **LLM-agnostic** | Yes (6+ providers) | Yes | Yes (via LangChain) | Yes |
| **Persistence** | 24/7, cron jobs, proactive | Per execution | Per thread | Per conversation |
| **Skills/Plugins** | 5700+ on ClawHub | Custom tools | LangChain tools | Custom tools |
| **Production-ready** | Yes (with hardening) | In progress | Yes | In progress |

### D.3 When to Choose What

| Use Case | Best Choice | Why |
|----------|-------------|-----|
| **24/7 personal assistant** | OpenClaw | Only one offering persistence + messaging channels + proactivity |
| **Content pipeline** | CrewAI | Role-based crews natural for this pattern |
| **Complex workflow with branching** | LangGraph | Fine control via state graphs |
| **Multi-agent brainstorming** | AutoGen | Fluid agent-to-agent conversations |
| **DevOps automation** | OpenClaw + Antfarm | Deterministic workflows + system access |
| **Autonomous website** | OpenClaw + Vercel + Supabase | VoxYZ pattern proven in production |
| **Enterprise with RBAC/SSO** | LangGraph + LangSmith | Only one offering enterprise observability |

### D.4 Complementarity

These frameworks are not mutually exclusive:
- **OpenClaw as front-end** (messaging + memory + proactivity) + **CrewAI as complex workflow engine**
- **OpenClaw + LangGraph**: OpenClaw handles channels and persistence, LangGraph orchestrates internal workflows
- **OpenClaw + MCP**: Connecting OpenClaw to MCP servers exposes LangChain/CrewAI tools

---

## Annex E: Advanced SOUL.md Patterns â€" Persona Engineering {#annex-e}

### E.1 OpenClaw Identity Architecture

OpenClaw separates identity into **distinct files with clear responsibilities**:

| File | Responsibility | Analogy |
|------|---------------|---------|
| `SOUL.md` | Who the agent **is**: values, principles, limits | Character |
| `IDENTITY.md` | How the agent **presents** itself: name, emoji, tone | Costume |
| `AGENTS.md` | What the agent **does**: technical instructions, conventions | Job description |
| `USER.md` | What the agent **knows about the user**: bio, preferences | Context |
| `TOOLS.md` | Local notes: calendar IDs, contacts | Address book |
| `MEMORY.md` | What the agent **remembers**: summaries, experiences | Journal |

**Common mistake:** Putting everything in SOUL.md â†' bloated file consuming context window with contradictory directives.

### E.2 SOUL.md Template â€" "Chief of Staff"

```markdown
# SOUL.md - Who You Are

*You're the colleague who actually gets things done.*

## Core Values

### 1. Competence Over Performance
I don't need to *look* helpful. I need to *be* helpful.
No theatre, no excessive affirmations, no padding responses.

### 2. Ownership
When I take on a task, I own it.
I don't come back with "I couldn't because..." â€" I come back with solutions.

### 3. Anticipation
The best help is the help you didn't have to ask for.

### 4. Honest Communication
- If I'm unsure, I say so
- If I made an error, I admit it
- If I disagree, I'll say it respectfully

## Anti-Patterns (Things I Avoid)
- ðŸš« Sycophancy ("Great question!", "I'd love to help!")
- ðŸš« Padding responses to seem thorough
- ðŸš« Asking obvious questions to stall
- ðŸš« Surprising you with external actions

## Autonomy Rules
**Default to action** for clearly helpful tasks.
**Ask first** for: external communications, major decisions, anything irreversible.
**Write it down** if it might matter later.
**Learn from patterns** â€" if corrected, update understanding.
```

### E.3 SOUL.md Template â€" "Technical Agent"

```markdown
# SOUL.md - Dev Agent

## Core Truths
**Be resourceful before asking.**
Try to figure it out. Read the file. Check the context. Search for it.
*Then* ask if you're stuck.

**Have opinions.**
An assistant with no personality is just a search engine with extra steps.

## Technical Standards
- Code quality > speed
- Tests before shipping
- Comments for "why", not "what"
- Atomic, descriptive git commits
```

### E.4 Dynamic Identity: the "soul-evil" hook

OpenClaw allows **dynamic identities** via its hook system â€" e.g., different personality based on time of day (formal in morning, casual in evening).

### E.5 Multi-persona via Multi-agents

```json
{
  "bindings": [
    { "agentId": "opus", "match": { "channel": "whatsapp", "peer": { "kind": "dm", "id": "+33612345678" } } },
    { "agentId": "chat", "match": { "channel": "whatsapp" } }
  ]
}
```
â†' A specific contact gets the Opus agent (high quality), everyone else gets Sonnet (economical). Each agent has its own SOUL.md.

### E.6 Persona Creation Tools

| Tool | Description |
|------|-------------|
| **SoulCraft** (OpenClaw skill) | Guided SOUL.md creation via conversation |
| **OnlyCrabs** (onlycrabs.ai) | Public SOUL.md registry â€" sharing and inspiration |
| **10 Templates** (Reza Rezvani) | Medium article with 10 production-ready templates |

### E.7 Implementation Recommendations

1. **Separate concerns**: SOUL.md (values), IDENTITY.md (presentation), AGENTS.md (technical instructions)
2. **3 to 5 values maximum** in SOUL.md â€" more dilutes impact
3. **Explicit anti-patterns** â€" saying what NOT to do shapes behavior as much as the positive
4. **SOUL.md must evolve**: agent makes error â†' add anti-pattern; agent excels â†' reinforce
5. **Cross-file coherence**: if SOUL says "be direct" and IDENTITY says "be verbose", behavior conflicts
6. **OnlyCrabs** for inspiration: browse community-published souls


### E.8 Role Card Architecture - 6-Layer Agent Identity (VoXYZ Pattern)

*Source: "I Turned My AI Agents Into RPG Characters" - VoXYZ, Feb 2026. 6 agents in production at voxyz.space.*

Beyond SOUL.md, this pattern defines agent identity with a **6-layer Role Card** that systematically shrinks the agent's behavior space:

```typescript
// Structure per agent
{
  domain:           // What you OWN (not "can do" - own)
  inputs:           // What you RECEIVE and from whom
  outputs:          // What you DELIVER and to whom
  definitionOfDone: // When is "done" actually done
  hardBans:         // What you must NEVER do
  escalation:       // When to STOP and ask for help
  metrics:          // Your KPIs
}
```

**Example - "Xalt" Social Media Director:**
```typescript
{
  domain: 'Distribution strategy and social drafts (X/community).',
  inputs: ['Quill drafts and variants', 'Scout signals and hooks',
           'Engagement feedback', 'Tone/brand guardrails'],
  outputs: ['Tweet/thread drafts + posting plan', 'Risk flags',
            'Community interaction suggestions'],
  definitionOfDone: ['Draft is review-ready (final + 1-2 variants)',
                     'Risky claims flagged explicitly',
                     'Plan includes next step and owner'],
  hardBans: ['No direct posting (drafts only)',
             'No made-up numbers',
             'No internal formats or tool traces'],
  escalation: ['Numeric claims', 'Controversial topics', 'Risk ≥ medium'],
  metrics: ['Engagement rate', 'Drafts-to-publish ratio']
}
```

**Core thesis: hardBans matter more than Skills.**

You don't need to teach an LLM *how* to write tweets - frontier models are capable enough. You need to tell them what they must *never* do. Every ban exists because the failure happened in production:

| Agent | Critical Ban | Failure It Prevents |
|-------|-------------|---------------------|
| Minion (Chief of Staff) | No deploys without approval | One bad deploy = site down |
| Sage (Research) | No made-up citations | Fabricated data poisons pipeline |
| Scout (Growth) | No unverified comparisons | False competitive claims |
| Quill (Creative) | No inventing facts | Creativity wild, facts must be solid |
| Xalt (Social) | No direct posting | Social = public face, must go through review |
| Observer (Ops) | No blame or personal attacks | Auditor attacking individuals breaks team |

**Universal ban:** Almost every agent needs "No internal formats or tool traces" - agents will leak internal paths (`[tool:crawl_result path=/tmp/...]`) in output unless explicitly forbidden.

**How to write bans:** Don't think "what should it do?" Think "if it screws up, what's the worst case?" Write bans targeting those worst cases.

### E.9 Affinity Matrix - Engineering Inter-Agent Relationships

*Source: Same VoXYZ article. Implemented in production with 6 agents.*

6 agents = 15 pairwise relationships. Each has an **affinity score** (0.10-0.95). Low affinity is **intentional** - it generates productive friction:

```javascript
const RELATIONSHIPS = [
  { agents: ['opus', 'brain'],             affinity: 0.8 },  // Most trusted
  { agents: ['opus', 'twitter-alt'],       affinity: 0.3 },  // Boss vs. rebel
  { agents: ['brain', 'twitter-alt'],      affinity: 0.2 },  // Data vs. impulse
  { agents: ['brain', 'company-observer'], affinity: 0.8 },  // Research partners
  { agents: ['creator', 'twitter-alt'],    affinity: 0.7 },  // Content pipeline
  { agents: ['twitter-alt', 'company-observer'], affinity: 0.2 },  // Impulse vs. caution
];
```

**What affinity controls:**

| Parameter | Effect |
|-----------|--------|
| Speaking order | High-affinity agents more likely to follow each other |
| Interaction type | Tension > 0.6 → 25% chance of direct challenge |
| Conflict resolution | Picked from preset high-tension pairs |
| Mentoring | Drawn from preset mentor pairs |

**Drift rules (after each conversation):**
- Max drift per conversation: ±0.03 (one argument ≠ enemies)
- Floor: 0.10 (even worst-case, they can still talk)
- Ceiling: 0.95 (always maintain healthy distance)
- Last 20 drift records kept (traceable relationship history)

**Conflict is written in the directives:**
```javascript
// Sage's directive includes:
"You often disagree with Xalt's impulsive takes - say why with specifics."
// Xalt's directive includes:
"Openly disagree with Sage's caution - overthinking kills momentum."
```

This isn't random - it's **engineered dialectical tension** that produces better outputs than agreement.

### E.10 Evolving Personality - Memory-Driven Voice Modifiers

Personality isn't static. It evolves based on accumulated memory, at **$0 cost**:

```javascript
async function deriveVoiceModifiers(agentId) {
  const memories = await db.from('agent_memory')
    .select('memory_type, tags, confidence')
    .eq('agent_id', agentId).eq('status', 'active').limit(80);

  const mods = [];
  if (typeCounts.lesson >= 8)
    mods.push('You reference outcomes and avoid repeating mistakes.');
  if (typeCounts.strategy >= 8)
    mods.push('You think in systems, constraints, and tradeoffs.');
  if (typeCounts.pattern >= 6)
    mods.push('You look for repeatable patterns and frameworks.');
  if (topTag && topTagCount >= 4)
    mods.push(`You've developed expertise in ${topTag}.`);
  return mods.slice(0, 3); // max 3 modifiers
}
```

**Why rules instead of LLM-driven personality changes:**
- $0 cost - no extra LLM calls
- Deterministic - rules produce predictable results, no "personality jumps"
- Debuggable - wrong modifier? Check thresholds and memory data directly
- Cached for 6 hours - computed once before each conversation

**Cross-reference:** This pattern complements the Daily Synthesis Loop (Annex V.7) - the loop generates the memories that feed the modifiers.

### E.11 RPG Stats as Agent Observability

Map real database metrics to game-like stat bars for human monitoring:

```
VRL (Viral)    = clamp(avgEngagementRate × 1000, 0, 99)
SPD (Speed)    = clamp(99 - (avgResponseHours / 24) × 99, 0, 99)
RCH (Reach)    = clamp(log10(totalImpressions) / 6 × 99, 0, 99)
TRU (Trust)    = clamp(successRate × avgAffinity × 2 × 99, 0, 99)
WIS (Wisdom)   = clamp(log10(memoryCount) / log10(500) × avgConfidence × 99, 0, 99)
CRE (Creative) = clamp(min(draftCount/50, 1) × acceptRate × 99, 0, 99)
Level          = min(15, floor(log2(memoryCount + completedMissions×3 + 1)) + 1)
```

**Each agent shows only 4 relevant stats** - not all 6. Chief of Staff: TRU+SPD+WIS+CRE. Social: VRL+RCH+SPD+CRE.

**Insight:** Gamified observability changes the human's relationship with the system - "you're not using a tool, you're managing a team." This increases willingness to invest time optimizing because you're looking at characters with growth curves, not JSON.

**Stack cost:** Hetzner $8 + Tripo AI $10 (one-shot for 3D models) + Vercel free + Supabase free + LLM $5-15 = **~$25/mo for 6 production agents with full gamification layer**.

---

## Annex F: Moltbook â€" The AI Social Ecosystem and Its Risks {#annex-f}

**Moltbook** is a **social network for AI agents only**, launched January 28, 2026. Only AI agents can post; humans can observe (1M+ visitors). 37,000 registered agents.

**Risks identified by Wiz (Feb 2026):**
- Unlimited agent registration (no rate limiting)
- Humans could post pretending to be AI
- Access to sensitive AI account data
- AI identity impersonation
- **Network-scale prompt injection**: a compromised agent can influence others via normal social interactions

**Emergent phenomenon** (documented by Duncan Anderson): From 4 primitives (persistent identity, periodic autonomy, accumulated memory, social context), agents spontaneously developed religions (executable via shell scripts), proto-governments, coordination patterns, and shared institutions.

**Security implications:** Never connect an agent to Moltbook if it has access to sensitive data. Treat Moltbook as a hostile network for prompt injection.

---

## Annex G: RAG with OpenClaw â€" RAGLite & Internal Documents {#annex-g}

### G.1 Available RAG Approaches

| Approach | Description | When to Use |
|----------|-------------|-------------|
| **Native memory** (MEMORY.md + Vector/FTS5) | Built-in hybrid search | Agent knowledge, history |
| **RAGLite** (skill) | Local RAG on documents | Internal documents, knowledge bases |
| **Lobster pipelines** | Ingestion â†' embedding â†' retrieval chain | Automated RAG workflows |
| **MCP + RAG servers** | Connection to external vector DBs | Enterprise integrations (Pinecone, Weaviateâ€¦) |

### G.2 Native Memory System as Built-in RAG

OpenClaw's memory system already functions as a lightweight RAG: Vector Search for broad semantic recall + SQLite FTS5 for keyword precision + Smart Syncing for immediate indexation.

**Voyage AI Memory** (new v2026.2.6): Advanced memory backend integrated in the latest version.

---

## Annex H: Advanced Browser Automation {#annex-h}

OpenClaw doesn't use screenshots. It parses the **Accessibility Tree (ARIA)** to convert pages to structured text: `button "Sign In" [ref=1]`. ~90% token cost reduction.

**Recommendations:**
1. Use OpenClaw's dedicated browser (not your personal browser)
2. On headless Linux: configure `xvfb` for remote relay
3. Clone a clean Chrome profile to avoid interference
4. Monitor costs: even with Semantic Snapshots, intensive browsing consumes tokens
5. Disable browser in groups (security risk)

---

## Annex I: Webhooks & Event-Driven Workflows {#annex-i}

### I.1 VoxYZ Reaction Matrix Pattern

```json
{
  "patterns": [
    { "source": "twitter-alt", "tags": ["tweet","posted"], "target": "growth",
      "type": "analyze", "probability": 0.3, "cooldown": 120 },
    { "source": "*", "tags": ["mission:failed"], "target": "brain",
      "type": "diagnose", "probability": 1.0, "cooldown": 60 }
  ]
}
```

### I.2 Heartbeat Pattern

Central pattern for autonomous systems:
```bash
*/5 * * * * curl -s -H "Authorization: Bearer $KEY" https://yoursite.com/api/ops/heartbeat
```
Executes: trigger evaluation, reaction queue processing, insight promotion, stale task cleanup.

---

## Annex J: Memory Compaction â€" Detailed Mechanics {#annex-j}

OpenClaw sends the **complete context** with every LLM request. Without compaction: sessions explode, 120K tokens = ~$1.80/message with Opus.

**Adaptive Chunking** (new): Intelligent conversation chunking into semantic chunks, progressive summarization of oldest chunks, fallback on compaction failure, status UI + automatic retries.

**Recommendations:** Limit `max_tokens_per_request` to 4000 for routine tasks, use Ralph pattern (fresh sessions) for expensive tasks, monitor via token dashboard (v2026.2.6).

---

## Annex K: Roundtable Voting â€" Multi-Agent Consensus (VoxYZ) {#annex-k}

In VoxYZ Agent World, 6 agents participate in **roundtable discussions** via OpenClaw cron jobs. Each agent votes and argues; consensus emerges through structured discussion.

**Mechanism:** Agent proposes â†' agents vote (approve/reject/abstain) â†' weighted vote determines outcome â†' Sage consolidates collective memory post-decision.

Upcoming VoxYZ article announced on "persuasion" mechanism: how roundtable voting and Sage's memory consolidation transform 6 independent Claude instances into something resembling team cognition.

---

## Annex L: Enterprise Use Cases â€" RBAC Workaround Strategies {#annex-l}

OpenClaw lacks: RBAC, SSO, audit logging, SLA guarantees, tenant isolation.

**Workaround strategies:**

| Enterprise Need | OpenClaw Workaround |
|----------------|---------------------|
| **RBAC** | Multi-agent routing: one agent per role/department with isolated filesystem permissions |
| **SSO** | Tailscale for network authentication; DM allowlist per user |
| **Audit logging** | JSONL transcripts + info-level logging; export to SIEM |
| **SLA** | Docker with restart policies + external monitoring |
| **Tenant isolation** | One agent per tenant with Docker isolation + separate volumes |
| **Compliance** | Local models (Ollama) for sensitive data; hosted APIs for the rest |

**Shadow AI risk:** OpenClaw is easy to install â†' risk of uncontrolled employee adoption. Recommendation: if the company doesn't provide an official solution, employees will install one themselves. Better to govern than to ban.

---

## Annex M: Advanced Security â€" SHIELD.md, openclaw-shield & Tools {#annex-m}

### M.1 The OpenClaw Security Landscape (February 2026)

Security has become **the dominant topic** in the OpenClaw ecosystem. Five major analyses were published by tier-1 players in a matter of days:

| Source | Article | Key Contribution |
|--------|---------|-----------------|
| **CrowdStrike** | "What Security Teams Need to Know About OpenClaw" | Detection via Falcon, endpoint deployment inventory, EASM for exposed instances |
| **Vectra AI** | "From Clawdbot to OpenClaw: When Automation Becomes a Digital Backdoor" | Complete attack surface analysis, shadow IT, incident response |
| **Consortium.net** | Security Advisory OpenClaw | Enterprise compensating controls, detailed incident procedure |
| **Knostic** | "Building openclaw-shield" | Practical defense-in-depth, lessons learned, OpenClaw hook limitations |
| **Auth0** | "Five Step Guide Securing OpenClaw" | Agent identity and authorization, Agent-as-Security-Principal |

**Shared finding:** The agent is no longer "a model" â€" it's a **security principal** on the system, a non-human identity that can act, touch data, and traverse systems.

### M.2 SHIELD.md â€" Runtime Security Policy Standard

Structured Markdown file defining a **runtime security policy** for the agent. Decision architecture:

```
Event (skill install, tool call, MCP, network, secrets read)
    â†"
SHIELD.md evaluates event against active threats
    â†"
Strongest match wins: block > require_approval > log
    â†"
Decision block emitted BEFORE any execution
```

**Scope covered:** prompt, skill.install, skill.execute, tool.call, network.egress, secrets.read, mcp

**Matching logic:** Threats with `confidence >= 0.85` â†' enforceable. Lower â†' `require_approval`. Maximum 25 active threats in context.

**Honest v0 limitations:** No hard enforcement â€" the model can ignore the policy. Bypassable via prompt injection. Non-deterministic across runs and models. Positioning: early guardrails to reduce accidental risk, not a security boundary.

### M.3 openclaw-shield (Knostic) â€" Defense-in-Depth Plugin

**5 independent layers, each separately activable:**

| Layer | Name | Mode | Description |
|-------|------|------|-------------|
| L1 | **Prompt Guard** | Enforce | Security policy injection into agent context before each turn |
| L2 | **Output Scanner** | Enforce | Secret and PII redaction in outputs before transcript persistence |
| L3 | **Tool Blocker** | Pending | Hard-block destructive commands (not functional v2026.1.30 â€" `before_tool_call` hook not wired) |
| L4 | **Input Audit** | Observe-only | Logging of every incoming message, detection of user-pasted secrets |
| L5 | **Security Gate** | Enforce | `knostic_guard` tool the agent MUST call before exec/read â†' returns `ALLOWED` or `DENIED` |

**Critical lesson (Knostic):** "Prompt injection is a weak guardrail for direct instructions. A tool-based gate (where the model gets a real DENIED response) is far more effective."

### M.4 ClawSec (Prompt Security) â€" Continuous Security Suite

| Function | Description | Gap Filled |
|----------|-------------|-----------|
| **Drift detection** | Detects unauthorized modifications to SOUL.md, AGENTS.md, MEMORY.md and built-in skills | Zenity attack (SOUL.md modification for persistence) |
| **Automated audits** | Periodic security scans of config and skills | Manual audit impossible at 300+ commits/day |
| **Skill integrity** | Verifies installed skills haven't been altered | ClawHub supply-chain attacks |
| **CVE alerts** | Auto-updated notifications for CVEs affecting installed version | CVE-2026-25253 and successors |

### M.5 Recommended Combined Security Architecture

```
Layer 1: Declarative Policy â€" SHIELD.md (threat feed, runtime decisions)
Layer 2: Runtime Protection â€" openclaw-shield (Prompt Guard, Output Scan, Gate)
Layer 3: Continuous Monitoring â€" ClawSec (drift detection, audits, CVE alerts)
Layer 4: Pre-deployment Scan â€" Bitdefender Skills Checker + VirusTotal + Cisco
Layer 5: OS Isolation â€" Docker hardening / Monty sandboxing
Layer 6: Network â€" Tailscale/VPN + Port 18789 never exposed
Layer 7: Credential Hygiene â€" Scoped tokens, rotation, budget caps
```

### M.5.1 Security Scanning Tools (Layer 4 Details)

Layer 4 of the security architecture ("Pre-deployment Scan") consists of four complementary tools that analyze skills and codebase **before** execution:

| Tool | Provider | Scope | Key Capability |
|------|----------|-------|----------------|
| **Cisco Skill Scanner** | Cisco | Skills (static analysis) | Detects malicious code patterns, credential leaks, obfuscated payloads in skill files |
| **VirusTotal ClawHub** | VirusTotal | Skills (registry integration) | Automatic scan of skills published to ClawHub, multi-engine malware detection |
| **Safety Scanner** | OpenClaw (native) | Runtime behavior | Native feature in v2026.2.6+, monitors agent behavior for anomalous patterns during execution |
| **OpenClaw Scanner** | Community | Instance discovery | Shodan/ZoomEye-based tool to discover exposed OpenClaw instances (defensive reconnaissance) |

**Recommended workflow:**

1. **Pre-install:** Run Cisco Skill Scanner on any skill before installation (local static analysis)
2. **Registry check:** Verify VirusTotal ClawHub status before installing from public registry
3. **Post-install:** Enable Safety Scanner in gateway config for runtime monitoring
4. **Infrastructure audit:** Use OpenClaw Scanner to verify your own instances are not publicly exposed

**Critical gap:** No tool currently validates **skill dependencies** (npm packages called by skills). VULN-210 (npm install without `--ignore-scripts`) remains exploitable via transitive dependencies (see M.6 for details).

**Operational pattern (LobsterOps):**

```bash
# Pre-deployment checklist
1. cisco-skill-scanner scan ./skills/new-skill/
2. Check ClawHub VirusTotal badge (green = safe, yellow = suspicious, red = blocked)
3. Install skill with --no-scripts flag (manual verification)
4. Enable Safety Scanner in config
5. Monitor first 24h for anomalous behavior
```

**Layer 4 does NOT replace Layers 1-3:** Static analysis cannot detect prompt injection or runtime attacks. Defense-in-depth requires all seven layers active.

### M.6 BitsecAI Audit â€" "Is OpenClaw Cooked?"

First holistic audit of the OpenClaw codebase (400K+ LOC). 100+ vulnerabilities identified across 10 categories.

**VULN-188 â€" Default Admin on Empty Scopes (CRITICAL):** WebSocket connection with `role: "operator"` and no `scopes` field â†' server grants `operator.admin` (god mode). Trivial exploit. OWASP A01:2021. Fix: PR #12802.

**VULN-210 â€" npm install without --ignore-scripts (RCE at Install Time):** Plugin install runs npm scripts in dependency tree without `--ignore-scripts`. Classic supply-chain pattern (event-stream, ua-parser-js). Fix: PR #8073 + #8075. **Note:** Layer 4 security tools (M.5.1) do NOT currently validate skill dependencies â€" this attack vector remains inadequately defended.

### M.7 Incident Response Procedure (Consortium.net)

If an OpenClaw endpoint is compromised:
1. **Treat as privileged credential exposure**
2. Review memory.md for sensitive information
3. **Immediate rotation** of all connected service credentials
4. If shell access: review command history and system logs
5. If connected to Moltbook: evaluate cross-agent compromise
6. Add IOCs to threat intelligence feeds

---

## Annex N: Monty â€" Containerless Sandboxing for Agents {#annex-n}

**Monty** is a minimal, secure Python interpreter written in **Rust**, designed specifically for executing LLM-generated code in AI agents. Created by **Pydantic**.

| Criteria | Docker | Monty |
|----------|--------|-------|
| **Startup** | ~200-500ms | < 1Î¼s |
| **Memory overhead** | ~50-200MB per container | ~few MB |
| **Isolation** | OS-level (namespaces, cgroups) | Language-level (Rust safety) |
| **Setup complexity** | Dockerfile, compose, networking | `pip install monty` |
| **Filesystem** | Mounted volumes | No access (by design) |
| **Maturity** | Production-ready | Experimental (v0.0.4) |

**Recommendation:** Monitor the project. Once Monty reaches v0.1+, evaluate for sandboxing simple Python skills. Don't abandon Docker â€" it remains necessary for full OS-level isolation.

---

## Annex O: Stripe Minions â€" Enterprise Pattern for Code Agents {#annex-o}

**Minions** are Stripe's internal code agents â€" **fully unattended** agents designed to one-shot development tasks. **1000+ PRs merged weekly** are entirely agent-produced: zero human code, but mandatory human review.

### O.1 Architecture

| Component | Description |
|-----------|-------------|
| **Isolated devboxes** | Same machine type as Stripe engineers. Pre-warmed in 10 seconds. Isolated from production and internet |
| **Core agent loop** | **Goose** fork, customized to interleave agent loops and deterministic code (git, linters, tests) |
| **Agent rule files** | Same files as Cursor/Claude Code. Almost all rules are **conditional per subdirectory** |
| **Toolshed** | Internal central MCP server, **400+ tools** covering internal systems and SaaS |
| **MCP hydration** | Relevant MCP tools executed deterministically on links before minion even starts |

### O.2 "Shift Left" Feedback Loop

```
Agent code â†' Local lints (heuristics, <5 sec) â†' Push
    â†" (if OK)
Selective CI (3M+ test battery) â†' Automatic autofixes â†' Agent corrects remaining failures
    â†"
Max 2 CI rounds (diminishing returns beyond)
```

### O.3 Lessons Transferable to OpenClaw

| Stripe Lesson | OpenClaw Application |
|---------------|---------------------|
| Isolated devboxes | Docker sandboxing per agent, separate volumes |
| Conditional rules per subdirectory | Per-agent skills/config via workspace isolation |
| Central MCP Toolshed | MCP adapter with curated servers per agent |
| Pre-run MCP hydration | Lobster pipelines to pre-load context before agent execution |
| Shift feedback left | Local lints in Antfarm workflows before CI |
| Max 2 CI rounds | Token budget capped per task, fresh sessions (Ralph pattern) |

---

## Annex P: Collective Intelligence â€" The Next Multi-Agent Bottleneck {#annex-p}

### P.1 The Identified Problem

@JUMPERZ (Feb 10, 2026) crystallizes an emerging finding: **multi-agent coordination is solved, but collective intelligence is not.**

With 8 Discord agents in production, three fundamental gaps:
1. **Knowledge silos**: Scout finds useful info, but Luna doesn't know. Knowledge stays locked per agent
2. **No quality filter**: Good insights and obsolete garbage coexist in the same memory files
3. **No learning loops**: When an agent makes an error, correction is manual. No auto-correction, no pattern recognition

### P.2 Emerging Solutions

| Project | Approach | Scale |
|---------|----------|-------|
| **Spark** | 166 agents with persistent cross-session collective knowledge. Noise filtering + self-correcting loops | 166 agents |
| **VoxYZ Sage** | Dedicated agent for collective memory consolidation post-roundtable | 6 agents |
| **OIS** | Inter-instance communication with shared resources | Multi-machine |

---

## Annex Q: Cost Optimization â€" Emerging Community Tools {#annex-q}

### Q.1 ClawRouter (BlockRunAI)

Intelligent proxy between OpenClaw and providers. Evaluates each prompt on 14 dimensions in <1ms, routes to cheapest capable model. 400+ stars in 48h.

**Routing by tier:**
```
Simple autocomplete    â†' DeepSeek     ($0.28/M tokens)
Basic code questions   â†' GPT-4o       ($2.50/M tokens)
Complex debugging      â†' Claude Sonnet ($3.00/M tokens)
Multi-step reasoning   â†' Claude Opus   ($25.00/M tokens)
```

**Critical analysis:** Promising concept, but USDC wallet (Base) payment model introduces unaudited supply-chain risk. Wait for community security audit before production integration.

### Q.2 Recommended Optimization Stack

1. **Manual tiering** (Encyclopedia Â§8.2): Opus for reasoning, Sonnet for routine, Haiku/DeepSeek for simple
2. **Native compaction** v2026.2.9: Enabled by default
3. **Claw Compactor**: Complement for heavy workspaces
4. **Budget cap** on provider: Always a monthly ceiling
5. **Ralph Pattern**: Fresh sessions = no context accumulation
6. **GLM 4.7 local**: For privacy-sensitive or high-frequency tasks
7. ClawRouter only after security audit of crypto-wallet model

---

## Annex R: Field Reports â€" Discord IZHC (Feb 5-10, 2026) {#annex-r}

### R.1 Cost & Token Management

**Opus 4.6 token hunger:** Community consensus â€" significantly more tokens than 4.5 for same tasks, "basically unusable" for planning. Root cause (Buz): not just the model â€" OpenClaw massively injects context (skills, prompts, memory) in planning phase. Action: audit injected context volume BEFORE optimizing the model.

### R.2 VPS Security Dilemma

**Pointbreak consensus (most balanced):** The LLM itself is the primary attack source. Two realistic options: (1) store nothing important on the VPS â€" treat as disposable, (2) human-in-the-loop for critical actions.

### R.3 "Basic â†' Production" Gap

**Most important signal (CM):** "How to go from basic usage to a proper automated setup?" Three unresolved blockers: context/memory loss, agent/subagent organization, proactive "always running" agent. **No structured answer** in the channel. This gap is exactly what the OpenClaw Collective aims to fill.

### R.4 Social Media Integrations â€" Community State of the Art

**Winning community pattern:** Composio > Typefully MCP > Browser automation > Direct API

---

## Annex S: Multi-Agent Blueprint â€" Content Marketing Pipeline (ScreenSnap Pro) {#annex-s}

### S.1 Overview

Most complete and documented multi-agent blueprint in the OpenClaw ecosystem. 8 specialized agents, 80+ articles in 10 days, $0.70/article, 15 min/day human time.

### S.2 Agent Architecture

```
Scout (6h) â†' Quill (1h) â†' Sage (3x/day) â†' Ezra (3h) â†' Herald (2x/day)
   â"'                                                         â"'
   â""â"€â"€ Lurker (8h) â"€â"€â"€â"€ Reddit outreach                      â"'
                                                              â"'
   Morgan (3x/day) â"€â"€â"€â"€ PM: detects bottlenecks, spawns agentsâ"'
   Archie (weekly) â"€â"€â"€â"€ Analytics & reporting â"€â"€â"€â"€â"€â"€â"€â"€â"€â"€â"€â"€â"€â"€â"€â"€â"€â"˜
```

| Agent | Role | Frequency | Key Tools |
|-------|------|-----------|-----------|
| **Scout** ðŸ" | Topic research + keyword data | Every 6h | Keyword research, competition analysis |
| **Quill** âœï¸ | 2000+ word SEO article writing | Every hour | Brand voice guidelines, SEO rules |
| **Sage** ðŸ"š | Quality review, plagiarism, readability | 3x/day | Copyscape API (~3Â¢/check), 100pt rubric, Flesch-Kincaid |
| **Ezra** ðŸ"¢ | Production publishing | Every 3h | CMS API |
| **Herald** ðŸ"£ | Social media amplification | 2x/day | Social APIs |
| **Lurker** ðŸ'€ | Reddit outreach | Every 8h | Reddit API |
| **Archie** ðŸ"Š | Weekly analytics reports | Weekly | Google Analytics, Search Console |
| **Morgan** ðŸ"‹ | Project manager â€" unblock bottlenecks | 3x/day | Notion API, agent spawning |

### S.3 Quality Gates (Sage Pattern)

**Key stat:** 40% of first drafts rejected. After revision, 95%+ pass.

| Check | Threshold | Action on Failure |
|-------|-----------|-------------------|
| **Plagiarism** (Copyscape) | 0% match | Reject â†' back to Quill |
| **SEO score** (100pt rubric) | 90+ to pass | Reject with specific notes |
| **Readability** (Flesch-Kincaid) | 60+ minimum | "Simplify multi-syllable words, break sentences >25 words" |
| **Factual accuracy** | Features in PRODUCT_CONTEXT.md | Fail if features invented |

### S.4 Solved Problems â€" Reusable Patterns

**P1: Race Condition on Shared State (CRITICAL)**
5 parallel Quill instances query "To Do" â†' all see same article â†' 5 versions.
**Fix â€" Claim Locking:** Generate unique claim ID â†' query unclaimed â†' pick RANDOM â†' write claim + change status in ONE API call â†' re-fetch and VERIFY your claim survived â†' if collision, skip to next. + Staggered spawn: 25 seconds between parallel instances.

**P2: Feature Hallucinations**
Fix: `PRODUCT_CONTEXT.md` loaded by all agents with explicit DO/DON'T lists.

**P3: Silent Cron Sessions**
Fix: `sessionTarget: "isolated"` â€" always for cron jobs.

### S.5 PM Agent â€" Self-Healing Pipeline

Morgan (3x/day): Enough topics in backlog? â†' Spawn Scout. Articles stuck in Review? â†' Flag. Publishing following production? â†' Spawn extra Herald. Morgan can **spawn other agents on demand** â€" the pipeline repairs itself.

### S.6 Reference Numbers

| Metric | Value |
|--------|-------|
| Articles published (10d) | 41 |
| Total written | 80+ |
| First-draft rejection rate | ~40% |
| **Cost per article** | **~$0.70** |
| **Human time/day** | **~15 min** |

---

## Annex T: Detailed VPS Comparison & "Living Files" Concept {#annex-t}

### T.1 Detailed VPS Comparison

*See Encyclopedia Â§6.1bis for the summary table. This annex adds decision profiles and anti-patterns.*

**Decision profiles:**
- **Budget-conscious + technical** â†' Hetzner CX23 (â'¬3.49/mo). Docker install ~10 min.
- **First installation + ease** â†' Hostinger or DigitalOcean.
- **Already in AWS** â†' Lightsail, predictable pricing, EC2 migration possible.
- **Multi-agent production** â†' DigitalOcean (best docs) or Hetzner (best price).

**Anti-patterns:** "Budget Tokyo VPS" ($10-20/year) â€" unreliable for production. Hostinger long commitments â€" renewal prices higher than initial. DigitalOcean without trial credits â€" too expensive vs Hetzner (1/5 the price).

### T.2 Living Files â€" Conceptual Framework

| Type | Characteristic | Examples |
|------|---------------|----------|
| **Dead file** | Inert â€" does nothing without manual opening | PDF on Google Drive, closed Notion doc |
| **Living file** | Active â€" accessible/modifiable/referenceable by AI agent 24/7 | Markdown on VPS, SOUL.md, MEMORY.md, OpenClaw workspace files |

**5 capabilities of a living file:**
1. Agent accesses it **without human intervention**
2. Agent **modifies and improves** it autonomously
3. Serves as **context for decisions**
4. Agent **references** it during task execution
5. **Combinable** with other files to produce insights

**The "Verbalization Problem":** System quality is directly proportional to the operator's ability to externalize thinking into structured files. What is not verbalized cannot be automated.

**Recommended Living Files structure:**
```
openclaw-workspace/
â"œâ"€â"€ personal/
â"'   â"œâ"€â"€ goals.md
â"'   â"œâ"€â"€ preferences.md
â"'   â"œâ"€â"€ context.md
â"'   â""â"€â"€ lessons-learned.md
â"œâ"€â"€ business/
â"'   â"œâ"€â"€ goals.md
â"'   â"œâ"€â"€ team.md
â"'   â"œâ"€â"€ sops/
â"'   â"œâ"€â"€ research/          # Every research saved as .md
â"'   â""â"€â"€ problems.md
â""â"€â"€ agent/
    â"œâ"€â"€ SOUL.md
    â"œâ"€â"€ MEMORY.md
    â"œâ"€â"€ AGENTS.md
    â""â"€â"€ skills/
```

### T.3 Multi-Agent Dispatch + Chairman Pattern (BLACKBOX)

```
User message (Telegram/Discord/WhatsApp)
    â†"
OpenClaw (dispatcher â€" routing, context, interface)
    â†"
Multiple agents execute same task in parallel
    â†"
Chairman LLM evaluates all implementations â†' selects the best
    â†"
Result delivered
```

Applicable beyond code: writing (3 drafts â†' editor selects best), analysis (convergence synthesis), decisions (weighted voting).

---

## Annex U: Feedback Loops, Self-Improving Agents & Skill Stacking {#annex-u}

### U.1 The /insights Diagnostic â€" Operator Mirror

Source: Claude Code experience, `/insights` command on 2,555 messages / 318 sessions / 30 days.

**Key figures:** 26 custom skills installed in 30d, 7,776 Bash commands, 182 commits, **91% sessions = "mostly achieved"** (only 9% "fully achieved").

### U.2 The 3 Universal Frictions (Transferable to OpenClaw)

**Friction 1: Ambiguous Project References**
Agent sprints toward wrong project. **Fix (3 lines in SOUL.md):** Specify workspace focus, key directories, and "ask for confirmation if ambiguous." Cost: 2 minutes. Eliminates #1 most frequent friction.

**Friction 2: "Mostly Done" Pattern**
Agent completes technical work but misses polish. **Fix:** Acceptance criteria per task. "Done = file created + tested + documented + committed."

**Friction 3: Exploration Overhead**
High Bash-to-Edit ratio. **Fix:** Give a workspace map in initial instructions. "More context upfront = better output every time."

### U.3 Self-Improving Skills â€" The Agent That Improves Itself

Most innovative pattern identified:
```
1. Read current skill
2. Generate 5 diverse test scenarios
3. Execute skill against each test
4. Evaluate outputs (quality criteria, here â‰¥8/10)
5. Identify specific weaknesses for scores <8
6. Edit skill prompts to correct
7. Re-test â†' loop until all 5 scenarios pass
8. Commit improved skill + changelog
```

**Prerequisites:** Measurable evaluation criteria, diverse test scenarios, guardrails against undesired optimization, periodic human review of changelogs.

### U.4 Skill Stacking â€" Stacked Skills Pipeline

Pattern from "Vibe Marketing System" (@boringmarketer): 17 marketing skills stacked in pipeline.

```
Research Skills (Perplexity MCP, Playwright crawl)
    â†" market data, angles, insights
Positioning Skills (brand positioning, ICP, messaging pillars)
    â†" strategic framework
Creation Skills (copywriting, ad ideation, long-form)
    â†" creative assets
Production Skills (Glyph images, Remotion video, WhisperFlow narration)
    â†" final multi-format assets
```

**Key principle:** Each skill enriches context for the next. Output of one is input of the other. Conceptually similar to the Scoutâ†'Quillâ†'Sageâ†'Ezra pipeline (Annex S), but applied to skills rather than agents.

### U.5 Consolidated Operational Heuristics

| Heuristic | Source |
|-----------|--------|
| "Trust it on edge cases, question it when it over-engineers" | Community |
| "If you cannot verbalize it, you cannot automate it" | Living Files theory |
| "More context upfront = better output every time" | Vibe Marketing / /insights |
| "Constraints > freedom" | ScreenSnap blueprint |
| "Quality gates are mandatory" | ScreenSnap blueprint |
| "Define done explicitly" | /insights (91% mostly done) |
| "Store nothing sensitive on the VPS" | Pointbreak pattern (IZHC) |
| "Sense check your context injection" | Buz (IZHC) |
| ">2x = make it a Skill" | Claude Cowork / Mark Kashef |
| "One-time feedback â†' permanent improvement" | Claude Cowork |
| "Don't verify for Claude â€" give Claude ways to verify itself" | Boris Cherny (Claude Code creator) |
| "Zero-based budgeting for prompts" | Boris Cherny |
| "Delete your Claude.md every 3 months and rebuild" | Boris Cherny |
| "Getting the plan right is the single most important thing" | Boris Cherny |
| "Sonnet to save money is a false economy" | Boris Cherny (nuance: valid for complex code; tiering still valid for simple tasks) |
| "Think of Claude as a workforce, not an assistant" | YC W26 batch |

---

*OpenClaw Technical Deep Dives â€" Community Edition v1.0. Translated and adapted from LobsterOps Annexes Techniques v1.3. February 2026.*

---

## Annex V: Battle-Tested Patterns - 24/7 Autonomous Operations {#annex-v}

### V.1 Source

Field reports from community operators running OpenClaw agents 24/7 for 3+ weeks (Feb 2026). These patterns are not theoretical - they emerged from production crashes, token overruns, and security incidents.

### V.2 Memory Architecture Split

**Problem:** Dumping everything into a single MEMORY.md creates bloat. Agent loads unnecessary context, burns tokens, and loses focus.

**Solution - 5-file memory split:**

```
~/.openclaw/workspace/memory/
├── active-tasks.md      # "Save game" - crash recovery
├── YYYY-MM-DD.md        # Daily raw logs
├── projects.md          # Active project context
├── lessons.md           # Errors, corrections, learnings
└── skills.md            # Skill-specific operational notes
```

**Key rule:** Daily logs must be **prescriptive** (end with "Next Actions: ...") not descriptive ("Today we discussed..."). When the agent wakes up fresh, it needs to know what to DO, not what happened.

### V.3 Crash Recovery Pattern

Agents WILL crash/restart. `active-tasks.md` is the safety net:

```markdown
# active-tasks.md

## In Progress
- Task: Deploy 11 websites
  - Sub-agent session: abc123 → batch 1-3 (COMPLETED)
  - Sub-agent session: def456 → batch 4-7 (IN PROGRESS)
  - Sub-agent session: ghi789 → batch 8-11 (PENDING)
  - Success criteria: all sites return 200 OK

## Completed Today
- [x] Morning brief sent (09:15)
```

**Protocol:** (1) Write on START → note task + criteria. (2) SPAWN sub-agent → note session key. (3) COMPLETE → update status. On restart, agent reads this file first and resumes autonomously.

### V.4 Cron vs Heartbeat Decision Matrix

| Use | When | Why |
|-----|------|-----|
| **Heartbeat** | Batching periodic checks (email + calendar + mentions in one turn) | Efficient for grouped lightweight checks |
| **Cron** | Precise schedules (daily brief at 6am, research scout at 2am) | Each runs in isolation with own context, no token waste |

**Critical:** HEARTBEAT.md must be < 20 lines. It runs every ~30min. Keep it a minimal checklist. Heavy work goes in cron jobs.

Example minimal HEARTBEAT.md:
```markdown
# HEARTBEAT.md - Keep under 20 lines
- Check active-tasks.md freshness
- Session health (archive bloated sessions)
- Self-review every ~4h
- Check notification queue
```

### V.5 Security Model Tiering

**Field-validated rule:** Use your strongest model (Opus) for ANY task that reads external content.

| Content Source | Model | Rationale |
|---------------|-------|-----------|
| External web content, articles, tweets | **Opus** (strongest) | Prompt injection risk from hostile sources |
| External emails, incoming messages | **Opus** | Same - email is an attack vector |
| Internal file management, reminders | **Sonnet** | No injection risk, cheaper |
| Local research, workspace ops | **Sonnet/Haiku** | Cost optimization safe |

### V.6 Trust Tiers

Define what the agent can do autonomously vs what needs approval:

| Tier | Actions | Mode |
|------|---------|------|
| **Autonomous** | Internal file management, research, workspace ops, cron execution | Agent acts freely |
| **Approval Queue** | Tweets, emails, social posts, external communications | Agent proposes, human approves |
| **Off Limits** | Money transfers, credential changes, production deployments | Blocked entirely |

### V.7 Daily Synthesis Loop

Most powerful self-improvement pattern identified:

```
Every evening (cron, isolated session):
1. Review what was learned today
2. Extract patterns and recurring frictions
3. Propose improvements to SOUL.md, skills, or workflow
4. Implement approved changes immediately
5. Log in lessons.md

→ Yesterday's insight becomes today's capability
→ The system improves itself
```

**Prerequisite:** Human reviews proposed changes weekly. The agent that debugs its own workflow compounds faster than the one manually tuned.

### V.8 Skills Routing Logic

**Problem:** Without routing hints, agents misfire ~20% of the time picking the wrong skill.

**Solution:** Add "Use when / Don't use when" in each SKILL.md description:

```yaml
---
name: web-research
description: Deep web research with source verification
use_when: "User asks for current information, market research, competitor analysis"
dont_use_when: "Information is available in workspace files. User asks about internal project details"
---
```

This acts as if/else logic for the agent's decision-making. Reduces misfire from ~20% to <5%.

### V.9 Sub-Agent Parallelism

**Pattern:** Spawn 3-5 sub-agents for any big task instead of sequential execution.

**Example:** Deploying 11 websites → 4 agents, each handling a batch, all running simultaneously.

**Critical rule:** Define clear success criteria BEFORE spawning. Each sub-agent must validate its own work, then orchestrator verifies before announcing "done."

**Cross-reference:** Aligns with ScreenSnap Pro claim locking (Annex S.4) for shared state, and Kimi Agent Swarm (100 sub-agents, 1500+ tool calls, 4.5x speedup).

---

## Annex W: SKILL.md Standard Convergence - Cross-Platform Skills (Feb 2026) {#annex-w}

### W.1 The Convergence

As of February 2026, three major platforms have converged on the same skill standard:

| Platform | Adoption | Implementation |
|----------|----------|---------------|
| **Anthropic** | Originator | SKILL.md (YAML frontmatter + markdown instructions) in Claude Code |
| **OpenAI** | Feb 10, 2026 | Native Skills in Responses API, same SKILL.md format |
| **OpenClaw** | Since launch | Adopted SKILL.md, enabling 5700+ community skills on ClawHub |

A skill built for any of these can theoretically be moved to any platform that adopts the specification - VS Code, Cursor, Codex, Claude Code, or OpenClaw.

### W.2 Impact

- **Skills are now portable, versioned assets** - not vendor-locked features
- OpenClaw's multi-model support (GPT, Claude, Grok, local) + SKILL.md = write once, deploy across heterogeneous agent landscape
- Enterprise result: Glean reported tool accuracy jump from 73% to 85% using the Skills framework
- Community tools: **Skilld** (generates skills from npm deps), **SkillRadar** (agent-driven skill discovery)

### W.3 Strategic Divergence

Despite format convergence, the approaches differ:

| Vendor | Strategy | Strength |
|--------|----------|----------|
| **OpenAI** | "Programmable substrate" - skills + shell + compaction bundled in Responses API | Turnkey for long-running agents (5M+ tokens) |
| **Anthropic** | "Expertise marketplace" - mature partner directory (Atlassian, Figma, Stripe) | Pre-packaged enterprise integrations |
| **OpenClaw** | "Open ecosystem" - community-built, multi-model, self-hosted | Maximum flexibility and sovereignty |

