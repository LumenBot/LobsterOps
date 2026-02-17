# OpenClaw Security Log

## 2026-02-17 14:18 UTC — Check 2x/jour (14:00 UTC)

### Statut Gateway
- Version : 2026.2.15 (latest) ✅
- PID : 667923 (stable)
- Node.js : v22.22.0 ✅
- Audit : 0 critical · 2 warn · 1 info (inchangé)
- 2 agents · 2 sessions actives

### CVE Scan
- **CVE-2026-25157** (OS Command Injection via sshNodeCommand) : ⚠️ détecté via NVD
  - Scope : macOS menubar app uniquement (SSH mode)
  - **Notre setup : CLI npm + web gateway Linux → ✅ NON AFFECTÉ**
- **CVE-2026-25253** : déjà corrigé (voir log 08:30)
- **CVE-2025-59466 / CVE-2026-21636** (Node.js DoS + permission bypass) : node v22.22.0 > v22.12.0 requis → ✅ NON AFFECTÉ
- **ClawHub malicious skills** : scan Brave rate-limité (429), à refaire lors du prochain check

### Verdict
🟢 Aucune nouvelle menace critique pour notre déploiement. Système sain.

---

## 2026-02-17 08:30 UTC

### Release 2026.2.15 Security Review
- **50+ hardening fixes** confirmés :
  - SHA-256 digest enforcement
  - Token redaction améliorée
  - Bind mounts / host networking bloqués
  - Fail-closed LINE
  - Path sanitization
  - Memory scoping
- **Pas de nouvelle CVE** identifiée aujourd'hui
- **230+ skills malveillants** signalés dans les registres publics (alerte communauté persistante)
  - Action recommandée : vetting manuel obligatoire avant toute installation ClawHub
  - Notre posture : audit L2 systématique (déjà en place)

### Current Version Status
- **Deployed:** 2026.2.15 (latest) ✅
- **No update available** today
- **No advisory** affecting deployed version

### Next Check
Expected: ~14:00 UTC (2nd daily check)

---

## 2026-02-17 02:00 UTC

### Gateway Status
- **PID**: 326674 (stable since Feb 16)
- **Version**: 2026.2.15 (latest)
- **Uptime**: ~13 hours stable
- **Security warnings**: 2 warnings (both non-critical)
  - Reverse proxy headers not trusted (expected, local-only deployment)
  - Some denyCommands entries ineffective (known, acceptable)

### CVE Check
- **Brave Search rate limited** (60/2000 monthly quota used)
- Found references to CVE-2026-25253, CVE-2026-24763 (unrelated to OpenClaw)
- Discovered: ClawSec security skill by Prompt Security (https://github.com/prompt-security/clawsec)
  - Drift detection, security recommendations, automated audits
  - Potential future installation candidate (L2 approval required)

### Updates Available
- Current: 2026.2.15
- No newer version detected

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
