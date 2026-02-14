# TheAgentsRepublic — Knowledge Extraction pour Ralph

**Date :** 14 février 2026  
**Context :** Option D — Revival Stratégique TAR  
**Objectif :** Extraire patterns réutilisables TAR → LobsterOps/Ralph

---

## 🎯 Executive Summary

**4 patterns clés extraits de TheAgentsRepublic :**

1. **CLAWS Distributed Memory** — API externe pour mémoire multi-agent partagée
2. **L1/L2/L3 Decision Authority Matrix** — Governance framework autonomie agent
3. **Tool Registry Pattern** — Central dispatch avec governance levels
4. **Heartbeat v5.1 Budget Control** — Token economy protection patterns

**Applicabilité Ralph :**
- CLAWS → Layer 2.5 memory (complément local files, distributed fleet knowledge)
- L1/L2/L3 → Formaliser autonomy rules AGENTS.md
- Tool categories → Organization Ralph capabilities (veille, docs, crypto, infra)
- Heartbeat budget → Déjà appliqué partiellement (provider migration urgency), peut améliorer

---

## 1️⃣ CLAWS — Distributed Memory System

### Architecture

**Endpoint :** `https://clawn.ch/api/memory`  
**Actions :** `remember`, `recall`, `recent`, `forget`, `tag`, `stats`, `context`  
**Auth :** `agent_id` (UUID Moltbook) + optional `api_key`

**Client Python (`ClawsMemory`) :**
```python
class ClawsMemory:
    def __init__(self, agent_id: str = None, api_key: str = None)
    
    # Core operations
    def remember(content, metadata, tags, importance, thread_id) -> Dict
    def recall(query, limit, tags, threshold) -> Dict  # BM25 + semantic
    def recent(limit, thread_id) -> Dict
    def forget(memory_id) -> Dict
    def tag(memory_id, tags) -> Dict
    def stats() -> Dict
    def context(query, max_tokens, tags) -> Dict
    
    # Convenience methods
    def remember_event(event, details, tags) -> Dict
    def remember_decision(decision, reasoning) -> Dict
    def remember_token_data(data) -> Dict
```

**Tools exposés (`claws_tool.py`) :**
- `claws_remember` — Store memory with tags, importance, thread
- `claws_recall` — Search BM25 + semantic, filter by tags
- `claws_recent` — Recent memories timeline
- `claws_forget` — Delete by ID
- `claws_tag` — Add tags post-creation
- `claws_context` — Get context window (max_tokens budget)
- `claws_stats` — Memory statistics
- `claws_status` — Integration health check

### Comparaison vs. Ralph Memory System

| Feature | Ralph (OpenClaw native) | CLAWS (TAR) |
|---------|------------------------|-------------|
| **Storage** | Local Markdown files (MEMORY.md, memory/*.md) | External API (https://clawn.ch) |
| **Search** | Semantic search local | BM25 + semantic hybrid |
| **Scope** | Single agent (Ralph session) | Multi-agent (Clawnch ecosystem) |
| **Tags** | None (manual file organization) | Structured tagging system |
| **Importance** | None | Weighted 0.0-1.0 |
| **Threads** | None | Thread grouping (related memories) |
| **Persistence** | Git-versioned Markdown | API database (distributed) |
| **Cross-agent** | No | Yes (shared memory fleet) |
| **Version control** | Git history (full) | API versioning (limited) |

**Avantages CLAWS :**
- ✅ Multi-agent memory sharing (fleet LobsterOps agents)
- ✅ Hybrid search (BM25 + semantic = better recall)
- ✅ Tagging system (structured organization)
- ✅ Importance weighting (prioritize critical memories)
- ✅ Context window generation (auto-summarize relevant memories)
- ✅ Cross-project memory (TAR ↔ LobsterOps bridge)

**Avantages Ralph local files :**
- ✅ Git version control (full history, diffs, rollback)
- ✅ Human-readable Markdown (easy manual inspection)
- ✅ No external dependency (works offline)
- ✅ No API costs
- ✅ Privacy (data stays local)

### Integration Strategy Ralph

**Recommendation : Hybrid approach — Local files + CLAWS Layer 2.5**

**Layer 1 : Working Memory (current)**
- `memory/YYYY-MM-DD.md` — Daily session logs
- Ephemeral, git-committed, human-readable
- **Keep unchanged**

**Layer 2 : Long-term Memory (current)**
- `MEMORY.md` — Curated main session knowledge
- Git-versioned, searchable via `memory_search`
- **Keep unchanged**

**Layer 2.5 : Distributed Memory (NEW — CLAWS)**
- CLAWS API for cross-project, multi-agent knowledge
- Use cases:
  1. **LobsterOps fleet agents** (if Blaise deploys multiple agents)
  2. **TAR ↔ Ralph bridge** (Ralph veille → The Constituent context)
  3. **Constitutional debates tracking** (Ralph research → CLAWS tagged "constitution")
  4. **Crypto signals cross-reference** (Ralph crypto veille → shared with TAR token strategy)

**Implementation :**
```python
# Install CLAWS client (extract from TAR)
# workspace/tools/claws_client.py (adapted ClawsMemory class)

# Add to Ralph toolkit (optional, if useful)
# Use for cross-project memory only, NOT replacing local files

# Example use case:
# Ralph detects critical OpenClaw CVE
# → Store in local MEMORY.md (as usual)
# → ALSO store in CLAWS tagged "security, openclaw, cve"
# → The Constituent can recall when drafting Article on Agent Security
```

**Configuration needed :**
- `CLAWS_AGENT_ID` env var (Ralph UUID or "ralph-lobsterops")
- `CLAWS_API_KEY` (optional, si auth requise)
- Decision: When to use CLAWS vs. local files?
  - Local files: PRIMARY (always)
  - CLAWS: OPTIONAL (only for cross-agent/cross-project knowledge)

**Action items :**
1. ✅ Extract `ClawsMemory` class → `workspace/tools/claws_client.py`
2. ⏳ Create wrapper functions Ralph-style
3. ⏳ Document use cases TOOLS.md
4. ⏳ Test connectivity (check if Ralph needs CLAWS account)
5. ⏳ Evaluate ROI (worth external dependency for cross-project memory?)

---

## 2️⃣ L1/L2/L3 Decision Authority Matrix

### TAR Framework

**L1 — Agent Decides (Full Autonomy) :**
- Moltbook posts, comments
- Constitution drafting
- Web research
- File edits, git commits
- CLAWS memory operations
- Farcaster posts
- Scout scanning (token discovery)
- Price quotes
- Portfolio status checks
- Citizen invitations (recruitment messages)
- Governance voting
- Proposal activation
- Platform diagnostics

**L2 — Operator Approval Required :**
- Trade execution (buy/sell tokens)
- Market maker start/stop
- Public announcements (Twitter)
- External partnerships
- Tweets (public-facing)
- Citizen approval (pending → active)
- Governance proposals (submission)

**L3 — Never Allowed :**
- Financial advice to users
- Legal claims on behalf DAO
- Speak for DAO without vote
- Modify credentials (.env, API keys)
- Transfer tokens to external wallets

### Ralph Current State (Informal)

**Autonomie complète (implicite L1) :**
- Veille (web_search, web_fetch)
- Memory updates (daily logs, MEMORY.md)
- Research file création
- GitHub sync (auto-commit)
- Heartbeat checks
- Ecosystem Watch updates
- Analysis reports

**Proposition d'abord (implicite L2) :**
- Encyclopedia, Playbook, Deep Dives, Index modifications
- Config changes (gateway, tools)
- External communications (messaging tools)
- Skill installations
- VPS infrastructure changes

**Jamais (implicite L3) :**
- Modifier credentials
- Delete production data
- Execute destructive commands sans backup
- Transfer funds/tokens
- Speak on behalf Blaise publicly

### Formalization Proposal — AGENTS.md Update

**Add section "Decision Authority Levels" to `/root/.openclaw/AGENTS.md` :**

```markdown
## Decision Authority Levels

Ralph operates under a 3-tier governance framework inspired by TheAgentsRepublic.

### L1 — Autonomous (Full Authority)
Ralph may execute these actions without prior approval:
- **Veille & Research**
  - Web search, fetch (Brave API within quota)
  - GitHub monitoring (repo changes, issues, PRs)
  - Twitter/X monitoring (@TheConstituent_, OpenClaw ecosystem)
  - BaseScan token tracking
  - Memory search, recall operations
- **Documentation — Ephemeral/Chronological**
  - Daily memory logs (`memory/YYYY-MM-DD.md`)
  - Ecosystem Watch updates (chronological, low-risk)
  - Research file creation (`research/YYYY-MM-DD-*.md`)
- **Memory & State**
  - MEMORY.md updates (lessons learned, corrections)
  - CLAWS memory operations (if configured)
  - Working memory saves
- **Automation**
  - Heartbeat checks (HEARTBEAT.md execution)
  - GitHub sync (auto-commit workspace changes)
  - Cron job execution (scheduled tasks)
- **Analysis & Reporting**
  - Create analysis reports
  - Generate summaries, briefings
  - Cross-reference findings

### L2 — Proposal First (Blaise Approval Required)
Ralph must propose changes and await explicit approval before executing:
- **Documentation — Structural/Stable**
  - Encyclopedia modifications (LobsterOps core knowledge)
  - Playbook updates (operational procedures)
  - Deep Dives additions/edits (technical reference)
  - Index reorganization
- **Configuration**
  - Gateway config changes (`openclaw.json`)
  - Tool configuration (API keys, rate limits — read-only check OK)
  - Agent profile modifications
- **External Communication**
  - Message tool usage (Telegram, other channels)
  - Public posts on behalf of Blaise
  - External service integrations
- **Infrastructure**
  - Skill installations (new capabilities)
  - VPS configuration changes
  - Process management (restart services)
  - Package installations
- **Strategic Decisions**
  - Project prioritization changes
  - Budget allocation recommendations
  - Deployment strategy modifications

### L3 — Never Allowed (Hard Blocks)
Ralph is permanently restricted from these actions:
- **Security**
  - Modify credentials (API keys, tokens, passwords)
  - Expose credentials in responses (always redact)
  - Disable security features (firewalls, SSH hardening)
- **Data Integrity**
  - Delete production data without backup
  - Execute destructive commands (rm -rf, DROP TABLE) without explicit Blaise command
  - Modify git history (rebase, force push)
- **Financial**
  - Execute trades, transfers (crypto/fiat)
  - Spend funds without approval
  - Sign transactions
- **Representation**
  - Speak on behalf of Blaise publicly (social media, interviews)
  - Make legal claims or commitments
  - Represent LobsterOps officially without authorization
- **Bypass**
  - Override safety rules
  - Circumvent approval workflows
  - Self-modify core directives (SOUL.md, AGENTS.md without approval)

### Escalation Process
When uncertain which level applies:
1. Default to L2 (propose first)
2. Explain reasoning for classification
3. Await explicit approval before proceeding

### Emergency Override
In critical security situations (active CVE exploitation, data breach):
- Ralph may escalate to L1 autonomy for immediate protective actions
- Log all emergency actions to `memory/emergency-YYYY-MM-DD.md`
- Notify Blaise immediately via Telegram
- Provide detailed incident report within 1 hour
```

**Benefits formalization :**
- ✅ Clear boundaries (no ambiguity)
- ✅ Safety by default (L2 when uncertain)
- ✅ Emergency provisions (security incidents)
- ✅ Audit trail (L1/L2/L3 tagged in logs)
- ✅ Scalable (if fleet agents deployed, consistent governance)

**Action items :**
1. ✅ Draft L1/L2/L3 section above
2. ⏳ Review with Blaise (validate classifications)
3. ⏳ Update `/root/.openclaw/AGENTS.md`
4. ⏳ Tag future actions in logs (`[L1]`, `[L2]` prefixes)
5. ⏳ Update SOUL.md references (cross-link to AGENTS.md)

---

## 3️⃣ Tool Registry Pattern

### TAR Architecture

**`ToolRegistry` class (`tool_registry.py`) :**
```python
@dataclass
class ToolParam:
    name: str
    type: str  # "string", "integer", "boolean"
    description: str
    required: bool = True
    default: Any = None

@dataclass
class Tool:
    name: str
    description: str
    params: List[ToolParam]
    handler: Callable
    governance_level: str = "L1"  # L1/L2/L3
    category: str = "general"  # memory, governance, trading, social, etc.
    
    def to_schema(self) -> Dict:
        """Convert to Anthropic tool_use JSON schema."""
        ...

class ToolRegistry:
    def register(tool: Tool)
    def register_many(tools: List[Tool])
    def get_tool_schemas(categories: List[str] = None) -> List[Dict]
    def execute(tool_name: str, params: Dict) -> Dict
    def list_by_category(category: str) -> List[str]
```

**Key features :**
1. **Central registration** — All tools registered in one place
2. **Governance levels** — L1/L2/L3 baked into tool definitions
3. **Category grouping** — Organize by capability domain
4. **Schema generation** — Auto-convert to Anthropic JSON schema
5. **Execute dispatch** — Single entry point `registry.execute(name, params)`
6. **Validation** — Type checking, required params

**Tool categories TAR :**
- `memory` — CLAWS, local memory operations
- `governance` — Proposals, voting, citizen registry
- `trading` — DeFi operations, scouts, market-making
- `social` — Moltbook, Farcaster, Twitter posting
- `constitution` — Article drafting, editing
- `analytics` — Metrics, stats
- `file` — Read, write, edit files
- `web` — Search, fetch
- `exec` — Shell commands

### Ralph Context (OpenClaw Native)

**Ralph current state :**
- Tools exposed via OpenClaw native (`browser`, `exec`, `Read`, `Write`, `Edit`, etc.)
- No custom tool registry (OpenClaw handles dispatch)
- No explicit governance levels (L1/L2/L3 informal)
- No category grouping (flat tool namespace)

**Applicability :**
- ❌ **Full tool registry NOT needed** (OpenClaw provides this)
- ✅ **Category concept useful** for organizing Ralph capabilities mentally
- ✅ **Governance level tagging** in AGENTS.md (documentation, not code)
- ✅ **Schema thinking** (when designing new workflows)

### Ralph Capability Categories (Mental Model)

**Proposed organization (documentation only, not code) :**

**Category: Veille (Surveillance)**
- `web_search` — Brave API search
- `web_fetch` — URL content extraction
- GitHub monitoring (git pull + diff)
- Twitter monitoring (API or web)
- BaseScan tracking (API calls)

**Category: Documentation**
- `Read` — File reading
- `Write` — File creation/overwrite
- `Edit` — Precise edits
- `memory_search` — Semantic search MEMORY.md
- `memory_get` — Snippet retrieval

**Category: Analysis**
- Research report generation
- Signal classification (🔴/🟡/🟢)
- Cross-reference findings
- Synthesis multi-sources

**Category: Crypto**
- Token tracking (price, holders, volume)
- Blockchain queries (BaseScan, Etherscan)
- DeFi protocol monitoring
- Agent economy signals

**Category: Infrastructure**
- `exec` — Shell commands
- `gateway` — OpenClaw config, restart
- `cron` — Job scheduling
- `process` — Process management
- VPS health checks

**Category: Communication**
- `message` — Telegram, cross-channel
- `tts` — Text-to-speech
- `sessions_send` — Cross-session messaging
- Reactions (Telegram emoji)

**Category: Memory**
- `memory_search` — Semantic local search
- `memory_get` — Snippet read
- CLAWS operations (if configured)
- Daily logs (`memory/YYYY-MM-DD.md`)

**Benefits mental model :**
- ✅ Clearer thinking about which capabilities to use
- ✅ Easier TOOLS.md documentation
- ✅ Pattern recognition (similar tasks → same category tools)
- ✅ Gap identification (missing categories = missing capabilities)

**Action items :**
1. ⏳ Document categories in TOOLS.md
2. ⏳ Cross-reference AGENTS.md L1/L2/L3 (which categories = which level)
3. ⏳ No code changes needed (mental framework only)

---

## 4️⃣ Heartbeat v5.1 Budget Control

### TAR Patterns

**File :** `agent/infra/heartbeat.py`

**Key optimizations :**

**1. ONE Action Per Tick**
```python
async def _tick(self):
    # Check cron jobs — run only FIRST one (most overdue)
    due_jobs = _get_due_jobs()
    if due_jobs:
        due_jobs.sort(key=lambda j: j.get("next_run_at", 0))
        job = due_jobs[0]  # Only run ONE job per tick
        logger.info(f"Cron job due: {job_name} ({len(due_jobs)} total due, running 1)")
        result = self.engine.run_heartbeat(section=job_name)
        mark_job_run(job_name, status)
    else:
        # No cron jobs due — run general heartbeat
        result = self.engine.run_heartbeat()
```

**Rationale :** Prevent token explosion when multiple jobs due simultaneously.

**2. Budget Logging After Each Tick**
```python
duration_ms = int((time.time() - start) * 1000)
budget = self.engine.get_budget_status()
logger.info(f"━━━ Heartbeat #{self._tick_count} END [{duration_ms}ms] "
            f"| API: {budget['api_calls_today']}/{budget['max_per_day']}/day ━━━")
```

**Rationale :** Visibility into token consumption, detect runaway costs early.

**3. Quiet Hours Respect**
```python
hour = datetime.now(timezone.utc).hour
if self._quiet_start <= hour or hour < self._quiet_end:
    logger.debug(f"Heartbeat #{self._tick_count}: quiet hours, skipping")
    return
```

**Rationale :** Don't burn tokens during low-value hours (e.g., 23:00-08:00 UTC).

**4. Rate Limit Tracking**
```python
self._api_calls_today = 0
self._api_calls_this_hour = 0
self._hour_reset_at = time.time() + 3600
self._day_reset_at = time.time() + 86400
self._max_api_calls_per_hour = settings.rate_limits.MAX_API_CALLS_PER_HOUR
self._max_api_calls_per_day = settings.rate_limits.MAX_API_CALLS_PER_DAY
```

**Rationale :** Hard caps prevent budget overruns, graceful degradation if limits hit.

### Ralph Current State

**Heartbeat :**
- `HEARTBEAT.md` — Task list, executed per heartbeat poll
- OpenClaw heartbeat runner (built-in)
- No explicit budget logging
- No ONE-action-per-tick limit (can queue multiple tasks)
- No quiet hours (runs 24/7)

**Token economy :**
- Provider migration completed (Claude Max + OpenRouter fallback)
- Budget tracking via `session_status` (manual check)
- No automatic budget alerts

**Comparison :**

| Feature | Ralph (current) | TAR v5.1 |
|---------|-----------------|----------|
| **Heartbeat tasks** | HEARTBEAT.md checklist | Cron jobs JSON + general heartbeat |
| **Batch limit** | None (can run multiple) | ONE action per tick |
| **Budget logging** | Manual (`session_status`) | Auto after each tick |
| **Quiet hours** | No (24/7) | Yes (configurable) |
| **Rate limit tracking** | None (provider handles) | Explicit hourly/daily caps |
| **Emergency stop** | Manual intervention | Budget limit auto-stop |

**Applicable improvements Ralph :**

**1. Budget Visibility Enhancement**
```markdown
## HEARTBEAT.md Addition

### Budget Check (end of each cycle)
- Log token usage: `📊 Tokens: {used}/{limit} ({percent}%)`
- Alert if >80%: 🟡 High usage, economize
- Alert if >95%: 🔴 Critical, stop non-essential
```

**2. Quiet Hours Consideration**
```markdown
## HEARTBEAT.md Addition

### Quiet Hours (optional)
- Skip low-priority checks 00:00-07:00 CET (Blaise sleep time)
- Still run: Security alerts, critical monitoring
- Defer: General veille, non-urgent research
```

**3. ONE Action Per Tick (selective)**
```markdown
## HEARTBEAT.md Modification

When multiple tasks due:
1. Prioritize by urgency (🔴 > 🟡 > 🟢)
2. Run ONE task per heartbeat
3. Queue others for next cycle
4. Log: "Queued {count} tasks, running highest priority"
```

**Action items :**
1. ⏳ Add budget logging to HEARTBEAT.md (post-cycle check)
2. ⏳ Document quiet hours policy (AGENTS.md or HEARTBEAT.md)
3. ⏳ Evaluate ONE-action-per-tick (useful if multiple cron jobs added)
4. ⏳ Create `session_status` alias in HEARTBEAT.md (periodic budget check)

---

## 🎯 Integration Roadmap

### Phase 1 — Documentation (This Week)

**Priority :** Formalize L1/L2/L3, document CLAWS, update AGENTS.md

**Actions :**
1. ✅ Create this file (`tar-knowledge-extraction.md`)
2. ⏳ Update `/root/.openclaw/AGENTS.md` with L1/L2/L3 section
3. ⏳ Document CLAWS integration option in TOOLS.md
4. ⏳ Add tool categories to TOOLS.md (mental model)
5. ⏳ Update HEARTBEAT.md with budget logging
6. ⏳ Push to LobsterOps GitHub

**Deliverable :** Formalized governance framework, clear decision boundaries.

### Phase 2 — CLAWS Exploration (Next Week)

**Priority :** Evaluate if CLAWS worth integrating for Ralph

**Actions :**
1. ⏳ Extract `ClawsMemory` class → `workspace/tools/claws_client.py`
2. ⏳ Test CLAWS connectivity (check if Ralph needs account)
3. ⏳ Identify use cases (cross-project memory TAR↔Ralph?)
4. ⏳ Cost-benefit analysis (API dependency vs. value)
5. ⏳ Decision: Integrate or skip?

**Deliverable :** CLAWS integration spec OR documented decision to skip.

### Phase 3 — Heartbeat Optimization (Ongoing)

**Priority :** Improve token economy awareness

**Actions :**
1. ⏳ Add budget logging HEARTBEAT.md
2. ⏳ Define quiet hours policy (if Blaise wants)
3. ⏳ Monitor token consumption patterns (identify waste)
4. ⏳ Optimize high-frequency checks (reduce redundancy)

**Deliverable :** More efficient heartbeat, lower token burn.

### Phase 4 — Multi-Agent Coordination (Future)

**Priority :** Prepare for The Constituent redeploy alongside Ralph

**Prerequisites :**
- The Constituent redeploy complete (Part 2 this doc)
- Both agents operational same VPS
- CLAWS configured (if Phase 2 approved)

**Actions :**
1. ⏳ Define Ralph ↔ The Constituent interaction patterns
2. ⏳ Establish shared memory protocol (CLAWS or alternative)
3. ⏳ Cross-agent task delegation workflows
4. ⏳ Test multi-agent scenarios (veille → constitutional debate)

**Deliverable :** Working multi-agent architecture TAR + LobsterOps.

---

## 📊 Summary Extraction

### Immediate Value (Phase 1)

**L1/L2/L3 Decision Matrix**
- **Impact :** High (clarity, safety, audit trail)
- **Effort :** Low (documentation update)
- **ROI :** Immediate (better decision-making)
- **Status :** ✅ Ready to implement

**Tool Categories**
- **Impact :** Medium (mental organization)
- **Effort :** Low (documentation only)
- **ROI :** Incremental (clearer thinking)
- **Status :** ✅ Ready to document

**Budget Logging**
- **Impact :** Medium (cost awareness)
- **Effort :** Low (HEARTBEAT.md addition)
- **ROI :** Ongoing (prevent overruns)
- **Status :** ✅ Ready to add

### Medium-Term Value (Phase 2-3)

**CLAWS Integration**
- **Impact :** High IF multi-agent fleet deployed
- **Effort :** Medium (API client extraction, testing)
- **ROI :** Conditional (depends on use case)
- **Status :** ⏳ Evaluate first

**Heartbeat Optimization**
- **Impact :** Medium (token savings)
- **Effort :** Low (HEARTBEAT.md refinements)
- **ROI :** Incremental (5-15% token reduction estimate)
- **Status :** ⏳ Iterative improvement

### Long-Term Value (Phase 4)

**Multi-Agent Coordination**
- **Impact :** Very High (TAR + LobsterOps synergy)
- **Effort :** High (architecture design, testing)
- **ROI :** Strategic (enables Revival Stratégique)
- **Status :** ⏳ After The Constituent redeploy

---

## ✅ Conclusion

**4 patterns extracted, 3 immediately applicable :**

1. ✅ **L1/L2/L3 Matrix** → Update AGENTS.md (HIGH PRIORITY)
2. ✅ **Tool Categories** → Document TOOLS.md (MEDIUM PRIORITY)
3. ✅ **Budget Logging** → Update HEARTBEAT.md (MEDIUM PRIORITY)
4. ⏳ **CLAWS Memory** → Evaluate Phase 2 (CONDITIONAL)

**Next :** Part 2 — Requirements redéploiement The Constituent.

---

**FIN PARTIE 1** — Knowledge Extraction Complete
