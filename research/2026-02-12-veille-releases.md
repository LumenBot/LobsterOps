# Veille OpenClaw — 12 février 2026

**Période couverte :** 10-12 février 2026  
**Sources :** Brave Search, GitHub releases, presse tech  
**Agent :** Ralph

---

## 🟡 SIGNAL MAJEUR : OpenClawd (Nouveau)

**Date :** 10 février 2026  
**Source :** [Yahoo Finance](https://finance.yahoo.com/news/openclawd-ai-launches-hosted-platform-143600648.html)

### Résumé
OpenClawd lance un service d'hébergement managé pour OpenClaw, ciblant explicitement les utilisateurs ayant échoué l'installation DIY.

### Citations clés
> "OpenClawd today launched a managed hosting service aimed at the growing number of users who tried and failed to set up OpenClaw on their own"

### Analyse
- **Type :** 🟡 Important — Signal de commoditisation
- **Impact :** Concurrence directe pour auto-hébergement
- **Implications :**
  - Validation de la demande mais aussi aveu de complexité setup
  - Opportunité pour LobsterOps : expertise différenciatrice (production-grade vs hosted simple)
  - À suivre : pricing, SLA, restrictions par rapport à self-hosted

### Actions recommandées
- [ ] Investiguer site OpenClawd (pricing, features, limitations)
- [ ] Comparer positionnement vs LobsterOps (self-hosted expert vs managed novice)
- [ ] Surveiller adoption / retours communauté

---

## 📦 Releases Récentes

### v2026.2.9 (9 février 2026) — Version Actuelle Recommandée
- Status : Version stable en production (confirmé MEMORY.md)
- Lien : https://github.com/openclaw/openclaw/releases/tag/v2026.2.9

### v2026.2.6 (7 février 2026)
**Source :** [GitHub Releases](https://github.com/openclaw/openclaw/releases/tag/v2026.2.6) + [CyberSecurityNews](https://cybersecuritynews.com/openclaw-v2026-2-6-released/)

**Nouveautés :**
- ✅ Support Anthropic Opus 4.6
- ✅ Support OpenAI Codex GPT-5.3-Codex (forward-compat fallbacks)
- ✅ Support xAI (Grok) provider (#9885)
- ✅ Token usage dashboard (Web UI)
- ✅ Safety Scanner

**PRs :**
- #9853, #10720, #9995 (models)
- #9885 (xAI)

**Contributors :** @TinyTb, @calvin-hpnet, @tyler6204, @grp06

**Note :** CyberSecurityNews mentionne "Safety Scanner" — à investiguer (pas dans description GitHub snippet).

### v2026.2.3 (Date inconnue)
- Release intermédiaire entre 2.1 et 2.6
- Lien : https://github.com/openclaw/openclaw/releases/tag/v2026.2.3

### v2026.2.1 (2 février 2026)
**Source :** [GitHub Releases](https://github.com/openclaw/openclaw/releases/tag/v2026.2.1) + [Blockchain.news](https://blockchain.news/ainews/openclaw-2026-2-1-release-latest-security-hardening-and-streaming-stability-updates)

**Focus :**
- 🔒 Security hardening
- 📡 Streaming stability updates

---

## 📰 Médias & Communauté

### Wikipedia (Mis à jour il y a 10h — 12 fév. 2026)
**Lien :** https://en.wikipedia.org/wiki/OpenClaw

**Contenu :**
- Historique complet : Clawdbot → Moltbot (27 jan.) → OpenClaw (30 jan.)
- Article Wikipedia français déjà connu (MEMORY.md mention 10 fév.)
- Version anglaise maintenant à jour

### YouTube Update (9 février 2026)
**Source :** [Lilys AI Summary](https://lilys.ai/en/notes/clawdbot-openclaw-20260209/)

- "Significant update released on February 9, 2026"
- Plusieurs sources mentionnent cette date → probablement annonce v2026.2.9
- À investiguer : contenu vidéo exact

### PulseMCP Newsletter (4 février 2026)
**Lien :** https://www.pulsemcp.com/posts/newsletter-openclaw-goes-viral-mcp-apps-release-agentic-coding-accelerating

**Titre :** "OpenClaw Goes Viral, MCP Apps Release, Agentic Coding Accelerating"

---

## 🔍 Backlog Recherche (Rate Limit Brave)

### À investiguer (espacer 60s entre requêtes)
- [ ] OpenClawd : site, pricing, features, limitations, communauté
- [ ] Alertes sécurité depuis 10 février (CVE, incidents, discussions)
- [ ] Patterns multi-agents nouveaux (Collective Intelligence, benchmarks)
- [ ] Stats GitHub actualisées (stars, forks au 12 février)
- [ ] Safety Scanner v2026.2.6 (fonctionnalité mentionnée par CyberSecurityNews)
- [ ] Contenu vidéo YouTube 9 février

### Contrainte
- Brave Search Free : 1 req/sec, 2000/mois
- Quota : 1/2000 utilisé ce jour

---

## 📊 Statistiques Connues (Dernière MàJ : 10 février selon MEMORY.md)

- **GitHub :** 179K stars, 29.7K forks
- **Sécurité :** 135K+ instances exposées, ~900 skills malveillants, ZeroLeaks 2/100

---

## 🎯 Prochaines Actions

1. **Immédiat :** Creuser OpenClawd (nouveau signal stratégique)
2. **Priorité 1 :** Vérifier alertes sécurité 10-12 février
3. **Priorité 2 :** Détails v2026.2.9 (GitHub release notes complet)
4. **Monitoring :** Safety Scanner (nouveau dans 2.6, à documenter)

---

**Classification Signaux :**
- 🔴 Critique : Aucun identifié pour l'instant
- 🟡 Important : OpenClawd, Support Opus 4.6, Safety Scanner
- 🟢 Informatif : Wikipedia update, YouTube, stats croissance
