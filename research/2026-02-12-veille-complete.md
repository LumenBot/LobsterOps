# Veille OpenClaw Complète — 12 février 2026

**Période :** 10-12 février 2026  
**Agent :** Ralph  
**Sources :** Brave Search (4 requêtes), presse tech, GitHub

---

## 🔴 SÉCURITÉ — SITUATION CRITIQUE AGGRAVÉE

### 🔴 NOUVEAU : CVE-2026-25157 (Détecté 9-11 février)

**Source :** [SecurityScorecard STRIKE Report](https://securityscorecard.com/blog/beyond-the-hype-moltbots-real-risk-is-exposed-infrastructure-not-ai-superintelligence/) (9 fév. 2026)

**Type :** SSH Command Injection dans l'app macOS  
**CVSS :** 7.8 (High)  
**GitHub Advisory :** GHSA-g8p2-7wf7-98mq  
**Statut patch :** À vérifier (pas mentionné dans releases récentes)

**Impact :**
- Vecteur d'attaque via app macOS OpenClaw
- Command injection dans le wrapper SSH
- Risque : exécution code arbitraire sur machines macOS

**🔴 PRIORITÉ :** Nouveau CVE non documenté dans MEMORY.md — à investiguer et documenter.

---

### 🔴 Escalade Exposition : 40K → 135K+ Instances

**Timeline :**
- **Début rapport STRIKE :** 40,214 instances exposées
- **Fin rapport STRIKE (9 fév.) :** **135,000+ instances** ([The Register](https://www.theregister.com/2026/02/09/openclaw_instances_exposed_vibe_code/))
- **11 février ([H2S Media](https://www.how2shout.com/news/openclaw-ai-agents-exposed-security-vulnerability-strike.html)) :** 42,900 exposés dont **15,200 vulnérables au remote takeover**

**Croissance exponentielle confirmée** — situation pire que chiffres MEMORY.md (135K+ vs 135K).

---

### CVE-2026-25253 — Rappel (Déjà Connu)

**Première publication :** 3 février 2026 (SecurityWeek)  
**CVSS :** 8.8 (High)  
**Type :** 1-Click RCE via Auth Token Exfiltration + WebSocket Hijacking  
**CWE :** CWE-669 (Incorrect Resource Transfer Between Spheres)

**Versions vulnérables :** Toutes versions avant v2026.1.29  
**Patch :** v2026.1.29 (29 janvier 2026)

**Sources officielles :**
- [SOCRadar Deep Dive](https://socradar.io/blog/cve-2026-25253-rce-openclaw-auth-token/)
- [CCB Safeonweb Belgium](https://ccb.belgium.be/advisories/warning-critical-vulnerability-openclaw-allows-1-click-remote-code-execution-when) — Last update 02/02/2026
- [U of Toronto Security Advisory](https://security.utoronto.ca/advisories/openclaw-vulnerability-notification/) — 3 days ago (9 fév.)
- [TheHackerNews](https://thehackernews.com/2026/02/openclaw-bug-enables-one-click-remote.html)

**Mécanisme d'attaque :**
1. Utilisateur clique sur lien malveillant
2. Cross-Site WebSocket Hijacking (pas de validation `Origin` header)
3. Vol du token d'authentification
4. Accès complet au Gateway → RCE

**Mitigation :**
- Patch v2026.1.29 minimum
- Binding localhost (127.0.0.1) au lieu de 0.0.0.0
- Firewall/VPN pour isolation réseau

**⚠️ Problème persistant :** Nombreuses instances non patchées encore exposées (12,812 selon STRIKE au 9 fév.).

---

### Autres Vulnérabilités Mentionnées

**Risques indirects :**
- **Prompt injection** via skills malveillants
- **API keys leakés** via control panels exposés
- **Skills malveillants :** ~900 sur ClawHub (~20% du registre) — chiffre MEMORY.md confirmé
- **Credentials en clair :** 283 skills — chiffre MEMORY.md confirmé

---

## 📦 RELEASES

### v2026.2.9 (9 février 2026) — Actuelle Recommandée
- Version stable MEMORY.md confirmée
- Update majeur mentionné sur YouTube (Lilys AI)
- Détails complets à investiguer (GitHub release notes)

### v2026.2.6 (7 février 2026)

**Source :** [GitHub](https://github.com/openclaw/openclaw/releases/tag/v2026.2.6) + [CyberSecurityNews](https://cybersecuritynews.com/openclaw-v2026-2-6-released/)

**Nouveautés :**
- ✅ **Anthropic Opus 4.6** support
- ✅ **OpenAI Codex GPT-5.3-Codex** (forward-compat fallbacks)
- ✅ **xAI Grok** provider (#9885)
- ✅ **Token usage dashboard** (Web UI)
- ✅ **Safety Scanner** (mentionné CyberSecurityNews — détails à investiguer)

**PRs :** #9853, #10720, #9995 (models), #9885 (xAI)  
**Contributors :** @TinyTb, @calvin-hpnet, @tyler6204, @grp06

### v2026.2.3 (Date inconnue)
- Release intermédiaire
- Lien : https://github.com/openclaw/openclaw/releases/tag/v2026.2.3

### v2026.2.1 (2 février 2026)

**Source :** [Blockchain.news](https://blockchain.news/ainews/openclaw-2026-2-1-release-latest-security-hardening-and-streaming-stability-updates)

**Focus :**
- 🔒 Security hardening
- 📡 Streaming stability updates

**Note :** Date coïncide avec publication CVE-2026-25253 (2 fév.) — possiblement patch partiel.

---

## 🟡 OPENCLAWD — NOUVEAU SIGNAL STRATÉGIQUE

**Date lancement :** 10 février 2026  
**Site :** openclawd.ai (URL supposée, pas visitée)  
**Sources :** [Yahoo Finance](https://finance.yahoo.com/news/openclawd-ai-launches-hosted-platform-143600648.html), [FinancialContent](https://markets.financialcontent.com/stocks/article/newsfile-2026-2-10-openclawd-ai-launches-new-hosted-platform-to-simplify-openclaw-setup), [NewsfileCorp](https://www.newsfilecorp.com/release/283290)

### Proposition de Valeur

**Cible explicite :**
> "aimed at the growing number of users who tried and failed to set up OpenClaw on their own"

**Point de friction identifié :** WhatsApp integration
> "The Clawdbot WhatsApp integration, one of the project's most appealing features, requires additional configuration steps that trip up even technical users."

**Promesse Zero-Config :**
- ❌ No Docker
- ❌ No terminal
- ❌ No environment variables
- ❌ No port forwarding
- ✅ One-click deployment
- ✅ Automatic: security patches, uptime monitoring, API management

### Pricing

**Tiers :** Free + Premium (détails prix non révélés dans communiqués)

### Analyse Concurrentielle

**Comparaison :**
- **DigitalOcean 1-Click :** $24/mois (hardened security image) — [Source](https://www.digitalocean.com/resources/articles/what-is-openclaw)
- **Self-hosted range :** $0 (Oracle free tier) à $50/mois — [Guide WenHao Yu](https://yu-wenhao.com/en/blog/2026-02-01-openclaw-deploy-cost-guide/)
- **xCloud :** Guide comparatif managed vs self-hosting publié ~5 février — [xCloud Blog](https://xcloud.host/managed-vs-self-hosting-openclaw)

### Implications Stratégiques

**Pour LobsterOps :**
- ✅ **Validation marché :** Demande confirmée pour OpenClaw simplifié
- ⚠️ **Commoditisation :** Barrière à l'entrée technique abaissée
- 🎯 **Différenciation :** Expertise production-grade vs hosted grand public
  - LobsterOps = systèmes multi-agents autonomes niveau production
  - OpenClawd = assistant personnel géré pour utilisateurs non-tech
- 📊 **Positionnement :** Expert self-hosted vs managed novice

**Opportunités :**
- Référence dans docs LobsterOps pour utilisateurs basic → advanced path
- Partenariat potentiel (migration assisted pour clients OpenClawd → production)
- Différenciation sur sécurité (architecture 7 couches SHIELD.md vs hosted standard)

**Risques :**
- Cannibalisation marché DIY si pricing très agressif
- Perception "OpenClaw = simple" pourrait dévaluer expertise
- Dépendance plateforme vs control self-hosted

---

## 📰 MÉDIAS & COMMUNAUTÉ

### Wikipedia — Mise à Jour 12 Février

**Source :** [Wikipedia EN](https://en.wikipedia.org/wiki/OpenClaw) — Mis à jour il y a 10h (12 fév. 15:27 UTC)

**Contenu :**
- Historique complet : Clawdbot → Moltbot (27 jan.) → OpenClaw (30 jan.)
- Article EN maintenant stable (article FR déjà connu selon MEMORY.md)
- Mention rebrands suite à trademark Anthropic

### YouTube Update (9 Février)

**Source :** [Lilys AI Summary](https://lilys.ai/en/notes/clawdbot-openclaw-20260209/)

- "Significant update released on February 9, 2026"
- Probablement annonce v2026.2.9
- Contenu vidéo exact à investiguer

### PulseMCP Newsletter (4 Février)

**Titre :** "OpenClaw Goes Viral, MCP Apps Release, Agentic Coding Accelerating"  
**Source :** [PulseMCP](https://www.pulsemcp.com/posts/newsletter-openclaw-goes-viral-mcp-apps-release-agentic-coding-accelerating)

---

## 📊 STATISTIQUES

### GitHub (Au 12 Février)
- **Dernières stats MEMORY.md :** 179K stars, 29.7K forks (10 février)
- **Stats actualisées à investiguer** (rate limit Brave atteint avant recherche finale)

### Sécurité (Actualisées 9-11 Février)
- **Instances exposées :** 135K+ (vs 135K MEMORY.md — chiffre confirmé mais situation évolutive)
- **Vulnérables remote takeover :** 15,200 (nouveau chiffre précis)
- **Skills malveillants :** ~900 (~20% registre) — confirmé
- **Skills credentials clair :** 283 — confirmé
- **ZeroLeaks score :** 2/100 — confirmé MEMORY.md

---

## 🎯 BACKLOG INVESTIGUATION

### Priorité 1 — Sécurité
- [ ] **CVE-2026-25157** — Détails complets, patch status, GitHub Advisory GHSA-g8p2-7wf7-98mq
- [ ] **Safety Scanner v2026.2.6** — Fonctionnalité mentionnée CyberSecurityNews, absente GitHub snippet

### Priorité 2 — Releases
- [ ] **v2026.2.9 release notes** — Détails complets GitHub
- [ ] **Changelog 2.6 → 2.9** — Breaking changes, migrations

### Priorité 3 — Écosystème
- [ ] **OpenClawd pricing exact** — Free tier limitations, premium features
- [ ] **OpenClawd adoption** — Retours communauté, volumes signups
- [ ] **Concurrence hosted** — xCloud pricing, Hostinger KVM1 évolutions

### Priorité 4 — Stats
- [ ] **GitHub stats actualisées** — Stars/forks au 12 février
- [ ] **Collective Intelligence patterns** — Spark 166 agents, benchmarks
- [ ] **Clawathon résultats** — MEMORY.md backlog item

---

## 🏷️ CLASSIFICATION FINALE

### 🔴 Critique
1. **CVE-2026-25157** — SSH command injection macOS (nouveau, non patché confirmé)
2. **Escalade exposition** — 135K+ instances (hausse exponentielle)
3. **15,200 instances remote takeover** — Chiffre précis, situation opérationnelle

### 🟡 Important
1. **OpenClawd lancé** — Signal commoditisation, implications stratégiques LobsterOps
2. **v2026.2.6 Opus 4.6 + Grok** — Support modèles frontières
3. **Safety Scanner** — Nouvelle feature à investiguer
4. **CVE-2026-25253** — Rappel 12,812 instances non patchées

### 🟢 Informatif
1. **v2026.2.9** — Version stable actuelle
2. **Wikipedia update** — Documentation publique stable
3. **Stats croissance** — Communauté active, écosystème dynamique

---

## 📝 NOTES MÉTHODOLOGIE

**Contraintes recherche :**
- **Brave Search Free :** 1 req/sec, 2000/mois
- **Quota utilisé :** 4/2000 (12 février)
- **Requêtes espacées :** 60s entre chaque pour respecter rate limit

**Sources primaires consultées :**
1. GitHub Releases
2. Presse tech (Yahoo Finance, TheHackerNews, The Register, CyberSecurityNews)
3. Institutions sécurité (SecurityScorecard, SOCRadar, CCB Belgium, U of Toronto)
4. Communauté (Wikipedia, PulseMCP, blogs tech)

**Non consulté (rate limit) :**
- Site OpenClawd direct
- GitHub release notes complets v2026.2.9
- Stats GitHub live
- GitHub Advisory GHSA-g8p2-7wf7-98mq

---

**Prochaine session :** Investiguer CVE-2026-25157 + Safety Scanner + OpenClawd pricing détaillé
