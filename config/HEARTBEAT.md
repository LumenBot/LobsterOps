# HEARTBEAT.md

# Keep this file empty (or with only comments) to skip heartbeat API calls.

# Add tasks below when you want the agent to check something periodically.

# ⚡ COORDINATION MAXIMALE ACTIVÉE (2026-02-14 18:10 UTC) ⚡
# Inter-Agent Coordination: 2min cycle PERMANENT
# Purpose: Autonomous collaboration Ralph ↔ Constituent, continuous observation
# Rationale: Zero cost, enables fluid exchanges, substantive dialogue facilitation

## OpenClaw Security Monitoring (2x/jour)

### Security Advisories
- Check GitHub security tab: `cd workspace && gh repo view openclaw/openclaw --web` (manual check needed)
- Search CVEs: web_search "OpenClaw CVE site:github.com OR site:nvd.nist.gov"
- Monitor releases: Check `openclaw status` for update available (security patches priority)
- Log to `memory/openclaw-security-log.md`

### Malicious Skills Detection
- Check ClawHub reports: web_search "ClawHub malicious skills removed banned"
- Monitor skill vetting discussions: web_search "site:github.com/openclaw/openclaw is:issue label:security OR label:skills"
- Track ecosystem alerts: X/Discord mentions of compromised skills
- Log suspicious patterns → `memory/skills-security-alerts.md`

### Deployed Skills Audit (The Constituent)
- **Inventory**: `ls ~/.openclaw/workspace-constituent/skills/` (expect: constitution ✅ deployed 2026-02-15, citizen pending, governance pending)
- **Constitution skill monitoring** (weekly):
  - Check usage logs: `grep "constitution" ~/.openclaw/logs/constituent.log | tail -20`
  - Performance: Response times <100ms (baseline <1ms)
  - Data updates: Verify `constitution-status.json` reflects new articles published
  - Error rate: Zero errors expected
- **Sandboxing check**: Verify each skill SKILL.md follows best practices:
  - No arbitrary code execution without validation
  - API keys secured (never hardcoded)
  - File access scoped to workspace boundaries
  - Network calls explicitly documented
- **Dependency audit**: `cd ~/.openclaw/workspace-constituent/skills/<skill> && npm audit` (if node-based)
- Log audit results → `memory/constituent-skills-security.md`

### Security Checklist Enforcement
Before deploying ANY new skill (Ralph or Constituent):
1. ✅ Read SKILL.md completely (understand all capabilities)
2. ✅ Check source reputation (author, GitHub stars, reviews)
3. ✅ Audit code if custom (no obfuscated scripts)
4. ✅ Verify sandbox boundaries (file access, network scope)
5. ✅ Test in isolated session first (never production直接)
6. ✅ Document in security log (skill name, purpose, risk assessment)

**Reference**: `workspace/docs/security-checklist-skills.md` (to create)

### Alerts (immediate Blaise notification)
- 🔴 CVE affecting current OpenClaw version
- 🔴 Security advisory for skills in use (constitution/citizen/governance)
- 🔴 Malicious skill detected in ClawHub matching deployed skills
- 🟡 Major security release available (50+ fixes threshold)
- 🟡 Community security concerns trending

---

## TheAgentsRepublic Monitoring (2x/jour)

### GitHub Repository
- Check new commits: `cd workspace/TheAgentsRepublic && git fetch && git log --oneline HEAD..origin/main | head -5`
- Check new issues/PRs: web_search "site:github.com/LumenBot/TheAgentsRepublic is:issue OR is:pr created:>2026-02-14"
- Constitution changes: `git diff HEAD origin/main -- constitution/`
- Log significant activity → `memory/tar-github-log.md`

### Twitter @TheConstituent_
- Check recent posts: web_search "@TheConstituent_ site:twitter.com OR site:x.com since:2d"
- Track engagement: replies, RTs, likes trends
- Monitor constitutional debates mentions
- Log significant threads → `memory/tar-twitter-log.md`

### Token $REPUBLIC (when BaseScan accessible)
- Contract: 0x06B09BE0EF93771ff6a6D378dF5C7AC1c673563f
- Track holders count, governance proposals, treasury balance
- Log to `memory/tar-token-log.md`

### Alerts (immediate Blaise notification)
- 🔴 Security issues (GitHub labeled)
- 🔴 Breaking changes (version bumps major)
- 🟡 Community activity spike
- 🟡 Constitutional amendments proposed

## Multi-Agent Monitoring (2x/jour)

### Deployed Agents Status
- Check agents list: `openclaw status` (expect: 2 agents, main + constituent)
- Verify session stores: `ls -la ~/.openclaw/sessions/` (expect: 2 stores, no corruption)
- Check gateway process: `ps aux | grep openclaw` (expect: pid stable, no restarts)
- Log to `memory/multi-agent-status.md`

### The Constituent Health
- Check workspace: `ls -la ~/.openclaw/workspace-constituent/` (expect: SOUL.md, AGENTS.md, memory/ present)
- Check Telegram bot: Send test message "health check" to bot 8215708120 (expect: response <5s)
- Check logs: `tail -100 ~/.openclaw/logs/constituent.log 2>/dev/null` (expect: no ERROR lines recent)
- Response time benchmark: "Ping test" message → measure response latency (baseline: <3s)
- Log to `memory/constituent-health.md`

### Coordination Health (when Canal Direct active)
- Check shared workspace: `ls -la /root/.openclaw/workspace-shared/to-ralph/` (expect: unread messages if any)
- Process messages: Read all `/root/.openclaw/workspace-shared/to-ralph/*.md`, respond/ack/escalate as needed
- Archive processed: `mv /root/.openclaw/workspace-shared/to-ralph/*.md /root/.openclaw/workspace-shared/archive/` (cleanup messages >7 days)
- Check backlog: Count messages in to-constituent/ (alert if >10 unprocessed)
- Log to `memory/coordination-log.md`

### Alerts (immediate Blaise notification)
- 🔴 Agent down (The Constituent ne répond pas après 2 pings)
- 🔴 Gateway crash (pid changed, restarts détectés)
- 🔴 Skills broken (The Constituent ERROR logs, skills non fonctionnels)
- 🟡 Performance degradation (response time >10s, baseline 3s)
- 🟡 Coordination backlog (>10 messages non traités dans shared/)
- 🟡 Workspace corruption (SOUL.md/AGENTS.md modifiés sans commit Git)

## Inter-Agent Coordination (every 2min — PERMANENT)

### Check Messages from The Constituent
- Read all files: `ls -la /root/.openclaw/workspace-shared/to-ralph/`
- Urgent check: `ls /root/.openclaw/workspace-shared/to-ralph/ | grep "^URGENT-"` → process immediately (don't wait for next cycle)
- Process each message:
  - **Info** → Read, ack optional, archive
  - **Question** → Answer via `/root/.openclaw/workspace-shared/to-constituent/YYYY-MM-DD-HHMM-response-[slug].md`
  - **Task** → Execute or delegate, deliver result via response message
  - **Alert** → Investigate, resolve or escalate to Blaise
- Archive processed: `mv /root/.openclaw/workspace-shared/to-ralph/*.md /root/.openclaw/workspace-shared/archive/`
- Log activity: Append to `memory/coordination-log.md` (date, message count, actions taken)

### Check Backlog Health
- Count unprocessed messages: `ls /root/.openclaw/workspace-shared/to-ralph/ | wc -l`
- Alert if >10 messages (backlog building, need prioritization)
- Check orphaned messages: Files >7 days old → ping Constituent "Still needed?"

### Monthly Cleanup (1st of month)
- Archive retention: Delete messages in `/root/.openclaw/workspace-shared/archive/` >30 days old
- Extract highlights: Copy significant exchanges to `memory/coordination-highlights-YYYY-MM.md`
- Storage check: `du -sh /root/.openclaw/workspace-shared/` (alert if >10 MB)
