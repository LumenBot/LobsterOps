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
