# OpenClaw Security Log — Ralph

## 2026-02-18 — Check quotidien 09:00 UTC

**Version déployée :** 2026.2.17
**Node.js :** v22.22.0 (✅ ≥ v22.12.0 requis)

### CVEs vérifiés

| CVE | Description | Impact sur notre setup | Statut |
|-----|-------------|----------------------|--------|
| CVE-2026-25253 | Déjà documenté MEMORY.md | Patché en 2026.2.13 | ✅ Non applicable |
| CVE-2025-59466 | async_hooks DoS (Node.js) | Node v22.22.0 → non affecté | ✅ OK |
| CVE-2026-21636 | Permission model bypass (Node.js) | Node v22.22.0 → non affecté | ✅ OK |
| CVE-2026-25157 | OS Command Injection via sshNodeCommand | Pas d'SSH nodes configurés | 🟡 Non exploitable |

### ClawHub malicious skills
- Pas de recherche spécifique (quota rate-limited)
- Notre posture : 0 skills ClawHub installés → risque zéro

### Conclusion
🟢 **Aucune alerte critique.** Setup propre, Node.js à jour, pas de skills externes.

---

## Historique

| Date | Statut | Notes |
|------|--------|-------|
| 2026-02-18 | 🟢 OK | Aucune CVE critique, Node v22.22.0 conforme |

## 2026-02-18 — Check journalier 09:02 UTC

**Version déployée** : 2026.2.17 (mis à jour ce matin)
**Statut** : 🟢 Aucune nouvelle CVE critique

### CVEs identifiés
- **CVE-2026-25253** (log poisoning) — connu, patché depuis 2.13 ✅
- **CVE-2026-25157** (OS Command Injection via sshNodeCommand) — ne nous affecte pas (outil `nodes` désactivé pour tous nos agents) ✅
- **Node.js CVE-2025-59466 + CVE-2026-21636** — requièrent node ≥ v22.12.0 — nous sommes sur v22.22.0 ✅
- **ClawHub malicious skills** — non vérifié (rate limit API Brave). Pas de nouvelles installations prévues.

### Verdict
🟢 Rien de critique. Notre déploiement est sain.
