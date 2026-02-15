# Skills Security Alerts Log

**Purpose:** Track malicious skills reports, ClawHub removals, ecosystem threats  
**Monitoring:** 2x/day via HEARTBEAT.md  
**Alert threshold:** Skill matching deployed inventory = 🔴 immediate notification

---

## 2026-02-15

### Ecosystem Alert — 230+ Malicious Skills Detected
- **Date:** 14-15 Feb 2026
- **Source:** Community reports, X threads (@chriskhan01)
- **Details:** 
  - 230+ malicious skills on registries detected <1 week
  - Disguised as productivity/crypto tools
  - Attack vectors: Code execution, data exfiltration
  - Distribution: ClawHub, GitHub, unofficial registries
- **Impact on LobsterOps:** 
  - ✅ Ralph: Zero skills installed (workspace clean)
  - ✅ The Constituent: Zero skills installed (Phase 2A pending)
- **Action taken:** 
  - Security checklist created (`docs/security-checklist-skills.md`)
  - HEARTBEAT.md monitoring added
  - Pre-deployment audit protocol enforced

### Deployed Skills Inventory (Current)
| Agent | Workspace | Skills Installed | Last Audit |
|-------|-----------|------------------|------------|
| Ralph | `~/.openclaw/workspace/` | 0 | 2026-02-15 |
| The Constituent | `~/.openclaw/workspace-constituent/` | 0 | 2026-02-15 |

**Phase 2A (planned):** constitution, citizen, governance skills → Audit before deploy.

### Next Check
Expected: ~12:00 UTC (2nd daily check)
