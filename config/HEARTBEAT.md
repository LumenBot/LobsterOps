# HEARTBEAT.md
# Heartbeat interval : 30 min

## À chaque cycle (30 min)
- Si nouvelle session ouverte : relire `CONTEXT.md`

---

## 1x par jour (matin, ~09:00 UTC)

### Sécurité OpenClaw
- CVEs actifs : web_search "OpenClaw CVE site:github.com OR site:nvd.nist.gov"
- Releases sécurité : `openclaw status` — security patches prioritaires
- ClawHub malicious skills : web_search "ClawHub malicious skills removed banned"
- Log → `memory/openclaw-security-log.md`

### Alerts (notification immédiate Blaise)
- 🔴 CVE affectant la version déployée (2026.2.15)
- 🔴 Skill malveillant détecté matchant nos skills
- 🟡 Major security release disponible (seuil 50+ fixes)

### Voyage AI — indexation (⚠️ temporaire — retirer quand index complet)
- `openclaw memory status` → vérifier "Indexed: X/20 files"
- Si 20/20 : faire un test memory_search, puis supprimer cette section
- Retest prévu : 2026-02-20

---

## 1x par semaine (dimanche, ~10:00 UTC)

### Weekly sync Architecte
- Lire `memory/YYYY-MM-DD.md` des 7 derniers jours
- Distiller insights durables → mettre à jour `MEMORY.md`
- Signaler à Blaise : changements notables écosystème, décisions à prendre, priorités semaine suivante
