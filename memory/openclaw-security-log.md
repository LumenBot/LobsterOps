# OpenClaw Security Log

**Purpose:** Track CVEs, advisories, security releases  
**Monitoring:** 2x/day via HEARTBEAT.md  
**Alert threshold:** CVE current version = 🔴 immediate Blaise notification

---

## 2026-02-16

### 🚨 RELEASE 2026.2.15 — SECURITY HARDENING (16 Feb 2026, LIVE)

**Current version**: 2026.2.14 → **Update recommended to 2026.2.15**

**Major Features**:
- Discord Components v2 (buttons, selects, modals for interactive prompts)
- Nested sub-agents (max depth configurable, default 2, max children 5)
- Per-channel ack reactions
- Cron webhook toggles
- LLM hooks exposed for extensions

**🔐 Security Fixes (50+)**:
- SHA-256 hashing (credential protection)
- Token redaction (logs sanitized)
- Sandbox restrictions (file boundary enforcement)
- XSS prevention (web dashboard hardening)
- Path sanitization (traversal attacks blocked)
- Runtime monitoring enhancements

**General Fixes**:
- Telegram streaming improvements
- Discord continuity fixes
- Memory scoping corrections
- Auto-reply mirroring fixes

**Breaking changes**: None

**🔴 ECOSYSTEM SECURITY CONCERNS (Persist)**:
- **230+ malicious skills** detected <1 week (code exec, data exfil)
- **26% of 31K skills vulnerable** (Cisco findings: data exfil + prompt injection)
- **Attack surface**: Wide open ecosystem, inadequate vetting
- **CVE-2026-25253**: Still critical concern (RCE one-click disaster)
- **Tens of thousands instances exposed** globally (China leads deployments)

**Mitigation strategy**:
- VirusTotal partnership (Feb 7) removing malicious skills from ClawHub
- Sandbox isolation improvements in 2.15
- Runtime monitoring for self-hosting
- Community patches fast-track

**🟡 FOUNDER NEWS**: Peter Steinberger joins OpenAI to build next-gen personal AI agent (OpenClaw continues open-source with OpenAI support)

**Action**: 🟡 Update to 2026.2.15 recommended (security hardening, no breaking changes)

**Deployed skills audit (The Constituent)**:
- 3 skills: constitution, citizen, governance (Phase 2A, Feb 15)
- All custom-built, security audited, zero external dependencies
- Risk: 🟢 Low (workspace-scoped, no network calls, L2 workflows for sensitive ops)

### Security Check 08:23 UTC
- **Version**: 2026.2.14 (Valentine's Day)
- **Update available**: 2026.2.15 🟡 **Security hardening recommended**
- **Gateway**: ✅ Stable (pid 88987, 44h+ uptime)
- **Constituent skills**: ✅ 3 operational (constitution, citizen, governance), custom-built, audited
- **Ecosystem risk**: 🔴 HIGH (230+ malicious skills, 26% vuln rate, CVE-2026-25253 active)
- **Action**: Monitor 2.15 release stability, plan update window

### Next Check
Expected: ~12:00 UTC (2nd daily check)

---

## 2026-02-15

### Release 2026.2.14 (Valentine's Day)
- **Date:** 14 Feb 2026
- **Security fixes:** 50+ hardening fixes
  - Sandbox isolation improvements
  - File boundary enforcement
  - 10 CVEs landed (@vincent_koc)
  - 40+ additional security patches
- **Breaking changes:** None
- **Action taken:** None (already on 2026.2.13, update available to 2026.2.14)
- **Status:** ✅ Monitored, no critical vulnerabilities current version

### Next Check
Expected: ~12:00 UTC (2nd daily check)
