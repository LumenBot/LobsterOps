# HEARTBEAT.md

# Keep this file empty (or with only comments) to skip heartbeat API calls.

# Add tasks below when you want the agent to check something periodically.

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
- Check shared workspace: `ls -la ~/.openclaw/workspace/shared/to-ralph/` (expect: unread messages if any)
- Process messages: Read all `to-ralph/*.md`, respond/ack/escalate as needed
- Archive processed: `mv to-ralph/*.md archive/` (cleanup messages >7 days)
- Check backlog: Count messages in to-constituent/ (alert if >10 unprocessed)
- Log to `memory/coordination-log.md`

### Alerts (immediate Blaise notification)
- 🔴 Agent down (The Constituent ne répond pas après 2 pings)
- 🔴 Gateway crash (pid changed, restarts détectés)
- 🔴 Skills broken (The Constituent ERROR logs, skills non fonctionnels)
- 🟡 Performance degradation (response time >10s, baseline 3s)
- 🟡 Coordination backlog (>10 messages non traités dans shared/)
- 🟡 Workspace corruption (SOUL.md/AGENTS.md modifiés sans commit Git)

## Inter-Agent Coordination (every 4h)

### Check Messages from The Constituent
- Read all files: `ls -la ~/.openclaw/workspace/shared/to-ralph/`
- Urgent check: `ls ~/.openclaw/workspace/shared/to-ralph/ | grep "^URGENT-"` → process immediately (don't wait for next cycle)
- Process each message:
  - **Info** → Read, ack optional, archive
  - **Question** → Answer via `to-constituent/YYYY-MM-DD-HHMM-response-[slug].md`
  - **Task** → Execute or delegate, deliver result via response message
  - **Alert** → Investigate, resolve or escalate to Blaise
- Archive processed: `mv ~/.openclaw/workspace/shared/to-ralph/*.md ~/.openclaw/workspace/shared/archive/`
- Log activity: Append to `memory/coordination-log.md` (date, message count, actions taken)

### Check Backlog Health
- Count unprocessed messages: `ls ~/.openclaw/workspace/shared/to-ralph/ | wc -l`
- Alert if >10 messages (backlog building, need prioritization)
- Check orphaned messages: Files >7 days old → ping Constituent "Still needed?"

### Monthly Cleanup (1st of month)
- Archive retention: Delete messages in `shared/archive/` >30 days old
- Extract highlights: Copy significant exchanges to `memory/coordination-highlights-YYYY-MM.md`
- Storage check: `du -sh ~/.openclaw/workspace/shared/` (alert if >10 MB)
