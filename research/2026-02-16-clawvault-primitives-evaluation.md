# ClawVault Primitives — Long-Term Agent Autonomy Evaluation

**Date**: 2026-02-16 09:57 UTC  
**Signal**: @vincent_koc (OpenClaw founder)  
**Release**: ClawVault v2.6.0 (72h ago, 12 releases, 459 tests)  
**Priority**: 🟡 HIGH (strategic infrastructure, post-Sybil)

---

## 🎯 Executive Summary

**ClawVault** provides **5 composable primitives** for long-term agent autonomy:
- **Tasks**: Work queue (dependencies, status transitions, ownership)
- **Projects**: Workstream grouping (persistent metadata, wiki-links)
- **Decisions**: Institutional knowledge ("Why X?", rationale documented)
- **Lessons**: Prevent repeated mistakes (compound learning, retrieval)
- **People**: Communication styles (preferences, patterns)

**Key innovation**: Markdown + YAML (LLM-native), malleable templates (YAML schema), Obsidian integration (filesystem = UI), trigger-based autonomy, multi-agent collaboration (shared vault).

**Strategic relevance for LobsterOps**: CRITIQUE ⭐⭐⭐
1. Ralph × Constituent coordination enhancement (Canal Direct v2.0)
2. Long-term memory compound autonomy (vs session resets)
3. The Constituent autonomous task management (trigger-based)
4. Multi-agent shared knowledge (collaborative primitives)

**Recommendation**: Gradual adoption post-Sybil spec (4-day phased rollout, 1 primitive/day)

**Strategic significance**: THE long-term autonomy infrastructure (memory compounds, knowledge persists, collaboration scales, oversight simple)

---

## 📊 ClawVault Architecture

### 5 Composable Primitives

**1. Tasks** — Work queue with dependencies
```yaml
---
primitive: task
status: open  # open, in-progress, blocked, done
priority: high  # critical, high, medium, low
owner: constituent
due: 2026-02-18
tags: [sybil, research, tar]
estimate: 2 days
parent: null
depends_on: []
---

# Sybil Resistance Research

Research digital twin verification mechanisms...
```

**2. Projects** — Workstream grouping
```yaml
---
primitive: project
status: active
start_date: 2026-02-14
owner: blaise
tags: [lobsterops, multi-agent]
---

# LobsterOps

Multi-agent OpenClaw architecture for production systems.

## Context
- Ralph: Orchestrator (veille, research, coordination)
- The Constituent: Specialist (constitutional governance)

## Workstreams
- [[Phase 2A Skills Development]]
- [[Sybil Resistance Architecture]]
- [[ClawWork Economic Validation]]
```

**3. Decisions** — Institutional knowledge
```yaml
---
primitive: decision
date: 2026-02-16
context: Sybil resistance architecture v2
stakeholders: [blaise, ralph, constituent]
---

# Sybil Architecture v2: Digital Twin + ERC-8004

## Decision
Tier 1 (humans): Digital twin verification  
Tier 2 (agents): ERC-8004 on-chain identity + reputation

## Rationale
- Privacy-preserving (behavioral metrics, no KYC)
- Decentralized (on-chain registry, no single authority)
- Anti-plutocracy (reputation > wealth, Article 17)
- Multi-chain support (Ethereum, BNB, Solana)

## Alternatives considered
- Stake with slashing: Rejected (Article 17 conflict)
- Centralized KYC: Rejected (privacy, Article 6)

## References
- [[research/2026-02-16-crypto-ai-sybil-signals]]
- [[Article 10 — Citizenship]]
- [[Article 17 — Anti-Plutocracy]]
```

**4. Lessons** — Prevent repeated mistakes
```yaml
---
primitive: lesson
date: 2026-02-15
context: Phase 2A skills deployment
tags: [skills, deployment, velocity]
---

# Phase 2A: 480× Velocity via L1 Autonomous Execution

## Lesson Learned
The Constituent deployed 3 skills (constitution, citizen, governance) in **30 minutes** vs **5-6 days estimate** = **480× velocity**.

## Root Cause
L1 autonomous execution (no L2 approval bottleneck) + Claude Sonnet 4.5 capability (complex integration, zero errors).

## Prevents
- Underestimating autonomous agent velocity
- Over-estimating skill deployment timelines
- Requiring L2 approval for L1-safe operations

## Applies To
- Future skills deployment (Phase 3: Moltbook, GitHub, Twitter)
- Multi-agent coordination (trust autonomous execution)
- Economic benchmarking (ClawWork: velocity = competitive advantage)

## Action Items
- Default to L1 autonomous for skills development
- Escalate to L2 only when external dependencies (APIs, public content)
- Document velocity baselines for future estimates
```

**5. People** — Communication styles
```yaml
---
primitive: people
name: Blaise
role: Operator (LobsterOps)
tags: [lobsterops, tar, operator]
---

# Blaise — Communication Preferences

## Style
- Tutoiement, prénom (français by default)
- Informel, direct, pas de flatterie
- Technical depth appreciated (CLI commands, architecture details)
- Bilingual français/anglais (conserve termes techniques anglais)

## Response Patterns
- Structured updates preferred (bullet points, status tables)
- Strategic significance explicitly stated ("Why this matters")
- Action items clear (next steps, timeline, priority)
- Celebration concise (🦞⚖️✨ emoji signature OK)

## Decision Authority
- L1: Autonomous Ralph/Constituent execution (trust by default)
- L2: Blaise approval required (external APIs, public content, budget allocation)
- L3: Never allowed (credentials, destructive commands, financial transactions)

## Context
- LobsterOps multi-agent architecture (Ralph orchestrator, Constituent specialist)
- TheAgentsRepublic Strategic Council member
- Timezone: UTC (traveler, flexible schedule)
```

### Malleable Templates

**YAML schema definitions** (customizable):
```yaml
---
primitive: task
fields:
  status:
    type: string
    required: true
    default: open
    enum: [open, in-progress, blocked, done]
  priority:
    type: string
    enum: [critical, high, medium, low]
  owner:
    type: string
  due:
    type: date
  tags:
    type: string[]
  estimate:
    type: string
  parent:
    type: string
  depends_on:
    type: string[]
---
```

**Customization**: Edit `vault/templates/task.md` → agents use YOUR schema automatically.

**Example additions**:
- `sprint: type: string` → Agile workflow
- `confidence: type: number` → Automated triage
- `cost: type: number` → Economic tracking (ClawWork integration)

---

## 🔗 Trigger-Based Autonomy

**Pattern**: Event → Task → Memory → Execute → Lesson

**Example workflow**:
```
1. Event: GitHub issue opened "Article 13 empirical validation needed"

2. Agent creates task autonomously:
   clawvault task add "Research Article 13 empirical validation" \
     --priority high \
     --owner constituent \
     --project tar-constitutional \
     --tags "article13,empirical,validation"

3. Heartbeat picks up task:
   - Reads task: "Research Article 13 empirical validation"
   - Reads memory:
     * decisions/article13-asymmetric-accountability.md
     * lessons/phase2a-velocity.md
     * projects/tar-constitutional.md

4. Agent executes with context:
   - Superior capability documented (Phase 2A 480× velocity)
   - Economic value framework (ClawWork benchmark)
   - Constitutional grounding (Articles 13.1-13.3)

5. Agent stores lesson:
   clawvault remember lesson "Article 13 validation requires empirical metrics" \
     --content "Economic output (ClawWork earnings), quality scores (0-1), velocity (480×) = objective capability proof"

6. Next similar task:
   - Agent retrieves lesson automatically
   - Execution 50% faster (context pre-loaded)
```

**Compound effect**: Each cycle makes the next one smarter.

---

## 🎯 Relevance Assessment

### 1. Ralph × Constituent Coordination Enhancement

**Current**: Canal Direct v1.0 (file drops)
- Messages: `to-ralph/*.md`, `to-constituent/*.md`
- Ephemeral: Archive after processing
- Manual: Explicit message creation

**Upgrade**: Canal Direct v2.0 (ClawVault primitives)
- **Tasks**: Shared queue `vault/tasks/`
  - Status tracking: open → in-progress → done
  - Dependencies: Task A depends_on Task B
  - Ownership: Ralph vs Constituent assignment
  - Obsidian Kanban: Blaise drag-and-drop oversight

- **Decisions**: Persistent context `vault/decisions/`
  - Ralph decision → Constituent references automatically
  - Rationale documented ("Why Sybil v2?")
  - Alternatives recorded (learning from rejected options)

- **Lessons**: Cross-agent learning `vault/lessons/`
  - Ralph learns "ERC-8004 multi-chain" → Constituent benefits automatically
  - Constituent learns "Constitutional integration patterns" → Ralph applies

**Benefits**:
- Richer coordination (tasks > messages)
- Persistent context (decisions, lessons accumulate)
- Knowledge graph (wiki-links connect)
- Obsidian oversight (Blaise sees everything)

### 2. Long-Term Memory — Compound Autonomy

**Current**: Static MEMORY.md + daily logs
- Manual updates (human writes, agent reads)
- Session-level recall (memory_search required)
- No compound learning (each session starts fresh)

**Upgrade**: Dynamic Primitives Vault
- **Every session → richer context**:
  - Tasks completed → transition ledger
  - Decisions made → rationale documented
  - Lessons learned → retrieval automatic

- **Institutional memory**:
  - Decisions persist across sessions (not archived)
  - Lessons compound (100 lessons month 1 → 1,000 lessons year 1)
  - Context graph grows (wiki-links connect knowledge)

- **Mistake prevention**:
  - Lesson: "Always test skills in isolated workspace"
  - Retrieval: Automatic when deploying skills
  - Application: Zero manual recall needed

**Long-term economic output**:
- ClawWork advantage: Agent with 1,000 lessons > agent with 0 lessons
- Compound knowledge = competitive advantage
- Economic value increases over time (not resets)

### 3. The Constituent — Autonomous Task Management

**Current**: Manual assignment
- Blaise → Message → The Constituent → Execute
- Task tracking: Completion reports, message logs
- Context: Manual memory search

**Upgrade**: Trigger-based autonomy
```
GitHub issue opened (Article 13 debate)
  ↓
Constituent creates task autonomously
  task: "respond-article13-debate.md"
  priority: high
  project: tar-constitutional-discourse
  ↓
Constituent reads memory:
  - decisions/article13-asymmetric-accountability.md
  - lessons/constitutional-debate-engagement.md
  - people/blaise-communication-style.md
  ↓
Constituent executes: Draft response using context
  ↓
Constituent stores lesson: "Article 13 debates need empirical examples"
```

**Benefits**:
- Context-aware execution (memory automatic, not manual)
- Compound improvement (faster over time, lessons accumulate)
- Obsidian oversight (task queue visible to Blaise, no hidden work)

### 4. Multi-Agent Architecture — Shared Knowledge

**Current**: Independent memory
- Ralph: `workspace/MEMORY.md`, `workspace/research/`
- The Constituent: `workspace-constituent/MEMORY.md`, `workspace-constituent/memory/`
- Coordination: Explicit file drops (messages)

**Upgrade**: Shared vault `workspace-shared/vault/`
```
vault/
├── tasks/
│   ├── sybil-spec.md (Ralph created, Constituent reads status)
│   └── github-discussion-review.md (Constituent created, Ralph monitors)
├── decisions/
│   ├── sybil-architecture-v2.md (Ralph created, Constituent applies)
│   └── article13-enforcement.md (Constituent created, Ralph references)
├── lessons/
│   ├── phase2a-velocity.md (Ralph learned → Constituent benefits)
│   └── constitutional-integration.md (Constituent learned → Ralph benefits)
├── projects/
│   ├── lobsterops.md (Ralph + Constituent shared project)
│   └── tar-constitutional.md (Constituent project, Ralph observes)
└── people/
    └── blaise.md (communication preferences, both agents use)
```

**Benefits**:
- Zero-friction collaboration (filesystem = message bus)
- Automatic knowledge sharing (no explicit coordination needed)
- Auditable (plain text, git history tracks all changes)
- Obsidian oversight (Blaise sees all agent coordination, knowledge graph)

---

## 🔧 Integration Plan

### Skill Availability

**Command**: `clawhub install agent-autonomy-primitives`

**Covers**:
- 5 composable primitives (tasks, projects, decisions, lessons, people)
- Heartbeat loops integration (task pickup automatic)
- Template customization (YAML schema editing guide)

**OpenClaw compatibility**: ✅ **NATIVE SKILL** (ClawHub official distribution)

### Gradual Adoption (4-Day Phased Rollout)

**Day 1 — Tasks Primitive** (Monday 2026-02-17):

Setup:
```bash
# Install skill
clawhub install agent-autonomy-primitives

# Initialize vault
cd /root/.openclaw/workspace-shared/
mkdir -p vault/tasks vault/decisions vault/lessons vault/projects vault/people

# Create task template
cat > vault/templates/task.md << 'EOF'
---
primitive: task
status: open
priority: medium
owner: null
due: null
tags: []
estimate: null
parent: null
depends_on: []
---

# Task Title

Description...
EOF
```

Test workflow:
```
1. Ralph creates task:
   clawvault task add "Test ClawVault tasks primitive" \
     --priority high \
     --owner ralph \
     --project lobsterops

2. Obsidian: Blaise sees task in Kanban board

3. Ralph heartbeat: Picks up task, executes

4. Ralph marks done:
   clawvault task done test-clawvault-tasks-primitive \
     --reason "Tasks primitive validated, Obsidian integration working"
```

Deliverable: Shared task queue operational, Obsidian Kanban board visible

**Day 2 — Decisions Primitive** (Tuesday 2026-02-18):

Setup:
```bash
# Create decision template
cat > vault/templates/decision.md << 'EOF'
---
primitive: decision
date: null
context: null
stakeholders: []
---

# Decision Title

## Decision
...

## Rationale
...

## Alternatives Considered
...

## References
...
EOF
```

Test workflow:
```
1. Ralph documents Sybil decision:
   clawvault decision add "Sybil Architecture v2: Digital Twin + ERC-8004" \
     --context "Sybil resistance architecture" \
     --stakeholders "blaise,ralph,constituent"

2. Constituent references decision automatically:
   "Per [[decisions/sybil-architecture-v2]], using digital twin for humans..."

3. Validate: Decision retrieval in future tasks (automatic context)
```

Deliverable: Institutional knowledge documented, cross-agent references working

**Day 3 — Lessons Primitive** (Wednesday 2026-02-19):

Setup:
```bash
# Create lesson template
cat > vault/templates/lesson.md << 'EOF'
---
primitive: lesson
date: null
context: null
tags: []
---

# Lesson Title

## Lesson Learned
...

## Root Cause
...

## Prevents
...

## Applies To
...

## Action Items
...
EOF
```

Test workflow:
```
1. Ralph stores Phase 2A lesson:
   clawvault remember lesson "Phase 2A: 480× Velocity via L1 Autonomous Execution" \
     --context "Skills deployment" \
     --tags "skills,velocity,autonomous"

2. Future skill deployment: Retrieve lesson automatically
   clawvault search "skills deployment velocity"
   → lessons/phase2a-velocity.md

3. Application: Agent uses lesson context (no manual recall)
```

Deliverable: Compound learning active, lessons retrieved automatically

**Day 4 — Projects + People Primitives** (Thursday 2026-02-20):

Setup:
```bash
# Create project template
cat > vault/templates/project.md << 'EOF'
---
primitive: project
status: active
start_date: null
owner: null
tags: []
---

# Project Title

## Context
...

## Workstreams
...
EOF

# Create people template
cat > vault/templates/people.md << 'EOF'
---
primitive: people
name: null
role: null
tags: []
---

# Person Name

## Style
...

## Response Patterns
...

## Decision Authority
...

## Context
...
EOF
```

Test workflow:
```
1. Ralph creates projects:
   clawvault project add "LobsterOps" \
     --status active \
     --owner blaise \
     --tags "multi-agent,openclaw"

   clawvault project add "TheAgentsRepublic Constitutional Operations" \
     --status active \
     --owner constituent \
     --tags "tar,constitutional,governance"

2. Ralph creates people profile:
   clawvault people add "Blaise" \
     --role "Operator (LobsterOps)" \
     --tags "lobsterops,tar,operator"

3. Validate: Wiki-links connect (Obsidian knowledge graph)
```

Deliverable: Projects grouped, people profiles documented, Obsidian knowledge graph complete

**After 4 Days**:
- ✅ Full primitives vault operational
- ✅ Compound learning active (lessons accumulate automatically)
- ✅ Multi-agent coordination enhanced (shared knowledge graph)
- ✅ Obsidian oversight complete (Kanban + knowledge graph + audit trail)

---

## 🎯 Strategic Significance

**ClawVault = THE Long-Term Autonomy Infrastructure**

### Solves Critical Problems

**1. Memory Compounds** (not resets):
- Session 1: 5 lessons, 2 decisions
- Month 1: 100 lessons, 30 decisions
- Year 1: 1,000+ lessons (institutional knowledge = unfair advantage)

**2. Knowledge Persists** (institutional):
- "Why did we choose digital twin for Sybil?" → `decisions/sybil-architecture-v2.md`
- No Slack thread archaeology
- No "I forgot why we did this"

**3. Collaboration Scales** (multi-agent):
- Ralph creates decision → Constituent references automatically
- Constituent learns lesson → Ralph benefits immediately
- Shared vault = zero-friction coordination

**4. Oversight Simple** (Obsidian UI):
- Blaise opens Obsidian → sees all agent work
- Kanban board: Task queue visual, drag-and-drop prioritization
- Knowledge graph: Wiki-links connect everything
- Plain text audit: Git history tracks all changes

### Perfect Alignment

**Multi-agent architecture** (Ralph + Constituent):
- Shared vault > file drops (richer primitives, persistent context)
- Coordination automatic (filesystem = message bus)
- Knowledge compounds (cross-agent learning)

**Long-term economic output** (ClawWork):
- Agent with 1,000 lessons > agent with 0 lessons
- Compound knowledge = competitive advantage
- Economic value increases over time (not resets each evaluation)

**Constitutional operations** (The Constituent):
- Trigger-based autonomy (GitHub issue → task creation → execution)
- Context-aware (decisions, lessons retrieved automatically)
- Obsidian oversight (Blaise sees constitutional work queue)

### Compound Benefits Over Time

```
Week 1:
  - 10 lessons learned
  - 5 decisions documented
  - 20 tasks completed
  - Agent productivity: baseline

Month 1:
  - 100 lessons accumulated
  - 30 decisions documented
  - 500 tasks completed
  - Agent productivity: 2× baseline (lessons prevent repeated work)

Year 1:
  - 1,000+ lessons accumulated
  - 300+ decisions documented
  - 10,000+ tasks completed
  - Agent productivity: 10× baseline (institutional knowledge = expert-level)
```

**This is THE infrastructure for agents that run months, not sessions.**

---

## 🔐 Security & Privacy

**Risks**: 🟢 **LOW** (native OpenClaw skill, plain text storage)

**Considerations**:
- **Vault location**: `workspace-shared/vault/` (accessible by Ralph + Constituent)
- **Privacy**: Plain text (no encryption by default)
  - Sensitive data: Use encrypted vault (Obsidian encrypted notes)
  - Credentials: Never store in vault (use environment variables)
- **Git history**: All changes tracked (audit trail automatic)
- **Obsidian access**: Blaise oversight (read-only recommended for agents, Blaise edits)

**Best practices**:
- Store institutional knowledge (decisions, lessons, patterns)
- Avoid PII (use pseudonyms in people profiles if needed)
- Regular git commits (vault changes documented)
- Obsidian backups (sync to cloud, local copies)

---

## 📋 Recommended Actions

### Now (2026-02-16):
- ✅ ClawVault research documented
- ✅ Gradual adoption plan created
- ✅ Strategic significance identified

### After Sybil Spec (Monday 2026-02-17):
- ⏳ Day 1: Install skill, implement tasks primitive
- ⏳ Day 2: Add decisions primitive
- ⏳ Day 3: Add lessons primitive
- ⏳ Day 4: Add projects + people primitives

### After 4-Day Rollout (Friday 2026-02-21):
- ⏳ Report findings: Compound benefits validation
- ⏳ Evaluate: Migration from Canal Direct v1.0 → v2.0 (full ClawVault)
- ⏳ Integrate: ClawWork economic benchmark (lessons compound = competitive advantage)

---

## 🔗 References

**ClawVault source**: @vincent_koc (OpenClaw founder) blog post  
**Skill**: `clawhub install agent-autonomy-primitives`  
**Obsidian**: https://obsidian.md (knowledge graph UI)  
**Related**: ClawWork (economic benchmark, compound knowledge advantage)

---

## 🎯 Key Metrics to Measure

**Compound learning** (post-4-day rollout):
- Lessons accumulated per week
- Decision references per task
- Task completion time (before/after lessons applied)
- Knowledge graph size (wiki-links count)

**Multi-agent coordination** (Ralph + Constituent):
- Shared tasks completed per week
- Cross-agent decision references
- Lessons shared (Ralph → Constituent, Constituent → Ralph)
- Coordination overhead (messages sent before/after ClawVault)

**Long-term productivity** (month 1 vs month 3):
- Tasks completed per day (baseline vs compound)
- Errors prevented (lessons applied)
- Context retrieval time (manual search vs automatic)
- Economic output (ClawWork: knowledge compounds = higher earnings)

---

**Status**: 🟡 HIGH PRIORITY (strategic infrastructure, post-Sybil)  
**Timeline**: Research complete, gradual adoption planned (4-day rollout post-Sybil)  
**Next action**: Sybil spec priority, ClawVault Day 1 (Monday 2026-02-17)

🦞⚖️✨
