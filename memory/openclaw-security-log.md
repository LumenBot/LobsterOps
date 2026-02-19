# OpenClaw Security Log

## 2026-02-19 08:42 UTC — Daily Check

**Version déployée** : 2026.2.17 (latest)  
**Node.js** : v22.22.0

### CVEs actifs

| CVE | Statut | Impact | Action |
|-----|--------|--------|--------|
| CVE-2026-25253 | 🟡 Connu | ZeroLeaks credential exposure | Mitigé (pas d'API keys dans SOUL.md, skill vetting manuel) |
| CVE-2025-59466 | ✅ Patché | Node.js async_hooks DoS | Node v22.22.0 > v22.12.0 requis |
| CVE-2026-21636 | ✅ Patché | Permission model bypass | Node v22.22.0 inclut fix |
| CVE-2026-24763 | N/A | Docker command injection | Pas de Docker sur ce déploiement |

### ClawHub malicious skills
- Aucun nouveau signalement détecté ce matin
- Skills installés (7) : tous auditables

### Releases
- **Latest** : 2026.2.17 (déployé ✅)
- Aucune alerte sécurité critique en attente

### Action requise
Aucune — déploiement à jour, CVEs critiques patchés.

---

## 2026-02-16 — CVE-2026-25253 Initial Report

135K+ instances exposées, ClawHub 20% malveillant, architecture 7 couches déployée (SHIELD.md).

---

*Log généré automatiquement via heartbeat quotidien.*
