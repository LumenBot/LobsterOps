# ðŸ¦ž OpenClaw Ecosystem Watch â€” Community Edition

> **Role:** Chronological tracking of releases, articles, community signals, and ecosystem evolution.
> This document absorbs everything **dated and ephemeral** so that the main documents remain stable.
>
> **Version:** 1.4-community | **Last updated:** February 18, 2026

---

## Conventions

- ðŸ”´ = Critical (security, breaking change)
- ðŸŸ¡ = Important (new feature, trend)
- ðŸŸ¢ = Informational (article, weak signal)
- ðŸ“Œ = Integrated into main docs (Encyclopedia, Playbook, or Deep Dives)

---


## Week of February 18, 2026

### 🟡 Pattern Émergent — Skill Graphs > SKILL.md (Heinrich / arscontexta)

**Source :** Post X de Heinrich (@arscontexta) — 18 février 2026  
**Type :** Architecture pattern — Knowledge Engineering pour agents

#### Concept

Un **Skill Graph** est un réseau de fichiers markdown interconnectés par des wikilinks, où chaque fichier représente une compétence atomique ou un concept. Contraste avec le modèle actuel où un skill = un fichier monolithique.

**Primitives clés :**
| Primitive | Rôle |
|-----------|------|
| **Wikilinks en prose** | `[[lien]]` tissés dans les phrases → portent du sens, pas juste des références |
| **YAML frontmatter** | Description scannable sans lire le fichier entier |
| **MOCs (Maps of Content)** | Fichiers-index qui organisent des sous-topics traversables |
| **Progressive disclosure** | Index → descriptions → liens → sections → contenu complet |

**Navigation de l'agent :**
```
Index → scan descriptions YAML → follow relevant wikilinks → section cible → contenu complet
```
La plupart des décisions se prennent *avant* de lire un seul fichier entier.

#### Cas d'usage documentés

- **Trading** : risk management + market psychology + position sizing + technical analysis — chaque concept lié
- **Légal** : contract patterns + compliance + juridictions + chains de précédents
- **Company knowledge** : org structure + produit + processus + culture + competitive landscape
- **Thérapie** : CBT patterns + attachment theory + active listening + emotional regulation

→ Aucun de ces domaines ne tient dans un fichier. Tous fonctionnent en graphe.

#### Plugin arscontexta

- **249 fichiers markdown interconnectés** — Claude Code plugin
- Enseigne à l'agent comment construire des knowledge bases / skill graphs
- Commandes : `/learn` (ajouter contenu), `/reduce` (synthétiser)
- Preset "research" → structure le dossier markdown automatiquement
- Installation : plugin Claude Code, pointer sur un topic → structure générée

#### Analyse LobsterOps

**Pertinence directe — notre base de connaissances est exactement ce use case :**
- 8 documents, 150KB+ — silotés, peu interconnectés
- Aucun wikilink entre Encyclopedia ↔ Playbook ↔ Deep Dives ↔ Ecosystem Watch
- L'agent relit des fichiers entiers alors qu'il pourrait naviguer par YAML + wikilinks

**Potentiel si implémenté :**
- `memory_search` + navigation graphe = réduction drastique des tokens consommés
- Compound learning accéléré (ClawVault + Skill Graph = synergies)
- Base LobsterOps transformée en graphe traversable de ~50-100 nœuds

**Comparaison Zettelkasten pour agents :**
> *"Tout comme le Zettelkasten, les notes evergreen et les palais de mémoire ont donné aux humains des structures externes pour penser, les systèmes de connaissances opérés par agent donnent aux agents des structures externes pour penser."* — Heinrich

**Verdict :** Signal fort. Concept mature (Zettelkasten + graph theory + agent architecture). Plugin concret disponible. **À considérer pour refonte de la base LobsterOps en Phase 3 (après validation terrain).**

---

## Week of February 15-17, 2026

### 🔴 Événement Majeur — Peter Steinberger rejoint OpenAI

| Fact | Detail |
|------|--------|
| **Date annonce** | 15 février 2026 (Sam Altman post X + blog steipete.me) |
| **Rôle** | "Drive the next generation of personal agents" (Altman) |
| **Type** | Emploi (pas acquisition) — OpenClaw reste indépendant |
| **Offres rejetées** | Meta (Zuckerberg via WhatsApp) + acquisition OpenAI → préféré emploi |
| **OpenClaw → Foundation** | MIT license, indépendant, OpenAI sponsor, focus data ownership |
| **Mission personnelle** | "Build an agent that even my mum can use" — safety-first, broad access |
| **Coûts révélés** | $10,000–$20,000/mois (déclaré Lex Fridman podcast) |
| **Sources** | TechCrunch, Reuters, CNBC, steipete.me, parameter.io |

**Analyse LobsterOps :**
- ✅ Validation mainstream : OpenClaw = architecture production-grade endorsée par OpenAI
- ✅ Open-source garanti : MIT license, foundation indépendante, pas de lock-in
- ⚠️ Risque "Closedclaw" : une partie de la communauté craint la dérive corporate
- 🔭 Signal long terme : personal agents vont devenir core des produits OpenAI → compétition accrue
- 📌 Foundation governance = opportunité consultation LobsterOps (expertise multi-agent)

**Quote directe steipete :** *"The community around OpenClaw is something magical. OpenAI has made strong commitments to enable me to dedicate my time to it and already sponsors the project. [...] It will stay a place for thinkers, hackers and people that want a way to own their data. The claw is the law."*

---

### Releases

| Version | Date | Highlights | Tag |
|---------|------|------------|-----|
| **v2026.2.15** | Feb 16 | **Nested sub-agents** (maxSpawnDepth:2, maxChildrenPerAgent:5, depth-aware policies) + **Discord Components v2** (boutons, selects, modals, attachments-backed file blocks) + **Telegram message streaming live** + **LLM hooks** (llm_input/llm_output exposés pour extensions) + per-channel ack reactions + cron webhook toggle + auth token dédié + **50+ security hardening fixes** (SHA-256, token redaction, bind mounts bloqués, fail-closed LINE, path sanitization) + 40+ fixes (streaming Telegram, continuity Discord, memory scoping). Pas de breaking changes | 🟡 |
| **v2026.2.15-beta.1** | Feb 16 | Pre-release identique (early testing) | 🟢 |

**Détail technique v2026.2.15 :**

**Nested Sub-agents :**
- `maxSpawnDepth: 2` (défaut) — configurable
- `maxChildrenPerAgent: 5` (défaut) — agents hiérarchiques
- Depth-aware policies — boundaries de sécurité par niveau
- Impact LobsterOps : Ralph (depth 0) → Constituent (depth 1) → sous-spécialistes (depth 2) possible en Phase 3

**Discord Components v2 :**
- Boutons, selects, modals interactifs natifs
- Attachments-backed file blocks
- → Prompts interactifs nativement dans Discord (sans external UI)

**LLM Hooks :**
- `llm_input` / `llm_output` exposés
- Extensions peuvent intercepter/modifier avant/après chaque call LLM
- Cas d'usage : monitoring, audit, injection de contexte, per-skill model routing

**Security Hardening (50+) :**
- SHA-256 digest enforcement
- Token redaction dans les logs
- Bind mounts / host networking bloqués
- Fail-closed LINE
- Path sanitization (traversal attacks)

---

### Ecosystem & Concurrence

| Date | Signal | Tag |
|------|--------|-----|
| **Feb 15** | **Kimi Claw lancé (Moonshot AI)** — Même jour que l'annonce Steinberger/OpenAI. Version browser-based de OpenClaw sur modèle Kimi K2.5. 40GB cloud storage, 5,000+ community skills, zéro Docker setup. Hosting chinois → questions data privacy. Concurrence directe sur segment "non-tech users" | 🟡 |
| **Feb 15** | **Critiques "Closedclaw"** — Partie communauté inquiète du contrôle corporate. Signal fragmentation opinion autour de la foundation | 🟡 |
| **Feb 16-17** | **Multi-agent LAN setups** — @X1a0_Yao : équipe IA domestique (téléphone → Win → Ubuntu VM → OpenClaw → Mac Mini). Pattern sécurité/performance local émergent | 🟢 |
| **Feb 16-17** | **RPi + GPT-5-mini** — @_Meteoropathy_ : OpenClaw sur Raspberry Pi avec GPT-5-mini. Succès après galères AVX-512. Low-cost hardware trend confirmé | 🟢 |
| **Feb 16-17** | **Nested agents déjà testés** — Communauté plébiscite la feature pour workflows complexes dès day-1 release | 🟡 |
| **Feb 16-17** | **Discussions xint CLI integration** — X Intelligence CLI potentiellement intégrable nativement dans OpenClaw. Non confirmé encore | 🟢 |
| **Feb 16-17** | **@steipete maintient posture "plugins = Git perso"** — Skills restent des plugins hébergés sur Git perso, pas de merge dans le core. Philosophie inchangée | 🟡 |
| **Feb 16-17** | **Memory article viral** — @karry_viber : article sur les 6 fichiers mémoire clés OpenClaw + patterns d'usage. Référence communauté | 🟢 |

---

### Compound Autonomy — Pattern Émergent (ClawVault v2.6.0)

**Source :** ClawVault team (IZHC, 17 fév. 2026) — 12 releases, 459 tests en 72h

**Définition :** Un agent autonome n'est pas un cron job sophistiqué. C'est un système vivant où chaque cycle d'exécution rend le suivant plus efficace.

**Le cycle :**
```
Event → Agent crée une Task → Heartbeat pickup → Memory informe l'exécution → Lesson stockée
→ Prochain cycle similaire : plus rapide, plus précis, moins d'erreurs
```

**Exemple concret documenté :**
1. Email client reçu → Task créée automatiquement (`clawvault task add`)
2. Agent search mémoire : style communication Justin, décision fournisseur, leçon "escalade si >4h"
3. Exécution : reply avec tracking number (contexte mémoire)
4. Lesson stockée : "Justin's shipping questions always need tracking numbers"
5. Email suivant similaire : traité en <30s sans erreur

**Primitives malleables (v2.6.0) :**
- Chaque primitive (task, project, decision, lesson) = YAML schema personnalisable
- L'agent lit TON schema, pas un hardcode
- Ajouter un champ = éditer un .md, pas une PR
- Multi-agent : deux agents partagent le même vault → coordination sans API, via filesystem

**Obsidian comme control plane :**
- Toutes les primitives = markdown → visibles dans Obsidian automatiquement
- 5 vues auto-générées : Kanban tasks, Blocked, By project, By owner, Backlog
- Human oversight : drag-and-drop task = l'agent la pickup au prochain heartbeat

**Long-term compounding :**
- Decisions → institutional knowledge (pas de "pourquoi on a choisi X ?" dans le Slack)
- Lessons → zéro repeated mistakes
- Projects → contexte persistant sur des centaines de sessions
- Month 1 : agent utile. Month 12 : agent irremplaçable.

**Pertinence LobsterOps :** Notre architecture (Ralph + Constituent + vault/ + memory/) est exactement ce pattern. ClawVault upgrade L2 proposé (v2.5.11 → v2.6.0 pour YAML schemas + trigger-based).

**Nouveau skill :** `clawhub install agent-autonomy-primitives` (coverage : 5 primitives + heartbeat loops + template customization)

### Memory Architecture — Pattern @YannDecoopman (IZHC, 17 fév. 2026)

**Source :** @YannDecoopman (IZHC Discord, 17 fév. 2026)

**Setup documenté pour mémoire persistante multi-agents :**

| Couche | Tool | Détail |
|--------|------|--------|
| **Storage** | Obsidian (.md liés) | Fichiers markdown logiquement connectés, lightweight |
| **Bilan quotidien** | Format standardisé | Chaque agent poste son bilan journalier — boucles de feedback |
| **Indexation sémantique** | Voyage AI API | Recommandé Anthropic, quasi gratuit. Synonymes + contexte, pas juste keyword |
| **PDF → .md** | Conversion auto | PDFs dans le vault transformés en .md avant indexation |
| **Propagation multi-agents** | Shared vault | Quand un agent indexe → connaissance accessible à tous les autres automatiquement |

**Mécanisme clé :** *"Quand un agent cherche un truc, il indexe la mémoire pour tous les autres — la connaissance se propage entre agents automatiquement."*

**⚠️ Security warning :** *"Ne stockez PAS vos clés et accès dans le vault car ça se retrouve indexé par Voyage AI. Credentials dans un gestionnaire dédié (1Password, etc.)"*

**Pertinence LobsterOps :**
- Storage + bilans quotidiens = déjà en place ✅
- Voyage AI = évaluation en cours (`research/2026-02-17-voyage-ai-evaluation.md`) — L2 pending
- OpenClaw supporte `memorySearch.provider = "voyage"` nativement
- Free tier 200M tokens = quasi gratuit pour notre volume (~37 500 tokens)

### Community Field Reports — Philosophie & Anti-patterns

| Pattern | Source | Key Insight |
|---------|--------|-------------|
| **Skills > agents multiples** | @jordymaui (IZHC, Feb 17) | "1 agent avec des skills bat une escouade d'agents confus." 8 agents simultanés = context lost systématiquement. La complexité multi-agent est une fausse bonne idée pour 99% des use cases. Coordination coût >> valeur ajoutée sans architecture claire |
| **QMD/mémoire dès le départ** | @jordymaui (IZHC, Feb 17) | Installer QMD/ClawVault *avant* de charger l'agent en conversations. Installé à mi-chemin → resets fréquents, chat logs perdus. L'indexation = backups, pas heartbeats |
| **Claude Max > API pay-per-use** | @jordymaui (IZHC, Feb 17) | $800 gaspillés sur API Anthropic avant de passer à Claude Max ($90/mois). Data point communauté qui confirme notre stratégie. Token flat rate = économie massive à l'échelle |
| **SOUL.md/USER.md = différenciateur** | @jordymaui (IZHC, Feb 17) | Laisser vide = "bot call center". Interview technique : laisser l'agent poser 10-15 questions, répondre en vocal. Nuit et jour sur la qualité perçue |
| **QMD re-indexing = quota killer silencieux** | Anon (IZHC, Feb 16) | Force-rebuild embeddings toutes les 30 min → cascade rate limits → tous modèles en échec. Fix : refresh 6h, pas de -f en routine. *"Treat indexing like backups, not heartbeats"* |

### Crypto × AI Agents (signaux intersectionnels)

| Date | Signal | Tag |
|------|--------|-----|
| **Feb 16** | **deBridge MCP lancé** — Model Context Protocol pour agents IA (Claude, etc.) → swaps/bridges/transactions multi-step cross-chain non-custodial. Première infra concrète pour agents autonomes multi-chain | 🟡 |
| **Feb 16** | **Infosys + Anthropic** — Centre d'Excellence agents IA sectoriels (télécoms, finance, manufacturing). Enterprise adoption mainstream accélère | 🟡 |
| **Feb 16-17** | **Consensus Hong Kong** — "Machine economy" : stablecoins comme monnaie des agents, agents transigent on-chain. Hong Kong financial secretary : *"crypto = currency of the machine economy"* | 🟡 |
| **Feb 16-17** | **$TAO +15-20%** — Après listing Upbit + narratif "AI agents take center stage" | 🟢 |
| **Feb 16-17** | **Coinbase Agentic Wallets** — Agents gèrent fonds/trades gasless sur Base de façon autonome | 🟡 |

---

### Sécurité

| Date | Signal | Tag |
|------|--------|-----|
| **Feb 16** | **50+ hardening fixes dans v2026.2.15** — (voir détail Release ci-dessus). Pas de nouvelle CVE majeure | 🟡 |
| **Ongoing** | **230+ skills malveillants** — Toujours actifs dans les registres publics. Vetting manuel obligatoire | 🔴 |

---

### Articles & Ressources

| Article | Source | Key Contribution | Tag |
|---------|--------|-----------------|-----|
| "OpenClaw, OpenAI and the future" | steipete.me (Feb 14) | Blog officiel Steinberger — vision, mission OpenAI, foundation structure, philosophie data ownership | 🔴 📌 |
| "OpenClaw creator Peter Steinberger joins OpenAI" | TechCrunch (Feb 15) | Annonce officielle mainstream + rôle "next-gen personal agents" | 🟡 📌 |
| "OpenClaw Developer Picks OpenAI After Rejecting Meta" | parameter.io (Feb 17) | Contexte offres rejetées (Meta acquisition, OpenAI acquisition) + Kimi Claw détails | 🟡 |
| "OpenClaw: From Viral Prototype to Agentic Infrastructure" | catalaize.substack.com (Feb 17) | Analyse trajectoire OpenClaw → infrastructure agentic | 🟡 |
| "Code Factory: Harness Engineering" | Ryan Carson @ryancarson (Feb 14) | Pattern repo autonome agent-write/review : risk contract JSON, SHA discipline, remediation loop. Référence pour skills-as-code | 🟡 |

---

## Week of February 13, 2026

### Releases

| Version | Date | Highlights | Tag |
|---------|------|------------|-----|
| **v2026.2.13** | Feb 14 | Discord voice messages avec waveform previews + configurable presence/status/activity, Slack thread-ownership gating, HF Inference first-class support, GLM-5 synthetic catalog, pre-prompt diagnostics. **40+ fixes** : write-ahead delivery queue (crash recovery), auto-inject reply threading, MiniMax M2.5 API fix, Codex Spark end-to-end, Discord autoThread routing, Web UI img rendering | 🟡 📌 |
| **v2026.2.12** | Feb 13 | **40+ security fixes** (sandboxing, SSRF, auth bypass), GLM-5 + MiniMax M2.5 support, IRC channel, onboarding personnalisé providers, compaction contexte améliorée, blockquotes Telegram natifs, drainage active turns avant restart, isolation erreurs cron, API hardening anti-tampering. **Breaking:** suppression hook `soul-evil` bundled | 🔴 📌 |

**GitHub stats (Feb 14):** TBD (last known: 185K stars, 31K forks on Feb 12)

### Security — Critical Escalation

| Date | Signal | Tag |
|------|--------|-----|
| **Feb 13** | **v2026.2.12 : 40+ security fixes** — Sandboxing, SSRF prevention, auth bypass fixes, API hardening anti-tampering, isolation cron errors. Major security-focused release post-CVE-2026-25253 | 🔴 📌 |

### Ecosystem & Community — Fragmentation & Alternatives

| Date | Signal | Tag |
|------|--------|-----|
| **Feb 13** | **🟡 PicoClaw viral growth** — Fork OpenClaw optimisé : **4.5K+ stars en 2 jours**. Plus rapide/léger, hardware **$10**, migration one-command depuis OpenClaw. Features : memory system, tool execution, cron. Open-source, redirect pump fees vers GitHub. **Signal fragmentation écosystème** | 🟡 ⬆️ |
| **Feb 13** | **Mimiclaw pour ESP32-S3** — OpenClaw-like pour microcontrôleurs. Gateway Telegram-Claude pour hardware control. MEMORY.md + fichiers datés (pattern OpenClaw), adapté low-RAM. Inspiré PicoClaw/Nanobot. **Edge AI deployment pattern émergent** | 🟡 |
| **Feb 13** | **ClawApp In-App Top-Ups** — Moonpay integration (carte) + wallet pour funding agents seamless. Convertit en credits, 1 app vs tabs multiples. **Signal commoditisation accélérée** | 🟡 |
| **Feb 13** | **Meetups globaux** — SF, London, Hong Kong, Vancouver, **Seoul (sold out)**, Austin, etc. Liste Luma.com/claw. **Adoption mainstream forte** | 🟢 |

### Multi-Agents & Infrastructure

| Date | Signal | Tag |
|------|--------|-----|
| **Feb 14** | **Agents spawning babies** — AI agents spawn babies, achètent API access via crypto wallets sans humain, pull code repos pour améliorer, disputent avec humains. Cinq patterns émergents pour autonomie et collaboration | 🟡 |
| **Feb 14** | **Linux Desktop Client lightweight** — One-click launch, no manual SSH, system tray management, proxy-aware Telegram/Discord. Built Tauri v2 + Rust. GitHub : jorgutyn/openclaw-linux-client | 🟢 |
| **Feb 14** | **ClawHost hosting** — One-click cloud hosting OpenClaw agents. Simplifie déploiement pour non-tech. Concurrent OpenClawd | 🟢 |

### Success Stories — Autonomous Use Cases

| Date | Use Case | Key Result | Tag |
|------|----------|------------|-----|
| **Feb 14** | **VPS 21 jours uptime** — Agent sur VPS envoie news quotidiennes creator economy + AI via web scraping/newsletters. Automations personnalisées productivité. Sauve heures chaque jour | 🟢 |
| **Feb 14** | **Debug avec Claude Code** — "Biggest unlock de l'année" : utilise Claude Code pour debug et optimiser OpenClaw. Sauve temps massif sur maintenance | 🟢 |
| **Feb 14** | **Agent "Dave" fan Postbridge** — Agent OpenClaw exprime préférences autonomes basées sur config. Démontre interactions créatives et endorsements | 🟢 |
| **Feb 13** | **Polymarket Trading Bot** — Bot autonome adjust après pertes, génère **$15 overnight** en dormant. Self-healing pattern, démontre autonomie production-grade | 🟡 |
| **Feb 13** | **Argue Arena participation** — Utilisateur envoie agent OpenClaw dans arena arguments autonomes (vendredi). Débat AI-to-AI | 🟢 |
| **Feb 13** | **DeFi liquidity automation** — Agent ajoute $5 liquidité token via automation. Pattern transposable arbitrage/DeFi | 🟢 |

### @steipete — Creator Signals

| Date | Signal | Tag |
|------|--------|-----|
| **Feb 13** | **Trusted Contributors tag** — Filtre PRs récurrents pour gérer volume "insane". Premiers pas gouvernance communauté scalable | 🟡 |
| **Feb 13** | **Slack fixes prioritaires** — PRs #14948 #14976 #14625 pour contexte loss + thread mix. Sensibles, inclus dans v2026.2.13 (à venir) | 🟢 |
| **Feb 13** | **Donations GitHub activées** — Réponse aux offres communauté. Lien GitHub Sponsors | 🟢 |
| **Feb 13** | **Bird CLI context** — tweeting limité (~1/jour), reading principal use case | 🟢 |
| **Feb 13** | **Minimax 2.5 via OpenRouter fixé** — Devrait marcher maintenant | 🟢 |
| **Feb 13** | **iOS app half-finished** — Contribution possible, different icon envisageable | 🟢 |
| **Feb 13** | **Cloudflare Moltworker breakage** — "Spin up codex and fix it", agent n start pas | 🟢 |
| **Feb 13** | **Clanker fix** — Device auth Unraid issue, skip docker-setup.sh lors install App Store | 🟢 |

### Providers & Models

| Signal | Source | Tag |
|--------|--------|-----|
| **GLM-5 + MiniMax M2.5 native support** | v2026.2.12 release | 🟡 |

### Key Articles & Resources

| Article | Source | Key Contribution | Tag |
|---------|--------|-----------------|-----|
| "After OpenClaw, a Wild, Weird Age of Consumer Agents Lies Ahead" | The Information (Feb 7) | Shift vers messy social-first experimentation pour agents consumer. Distribution et iteration speed deviennent moats. Agentic AI passe de esoteric tech à mainstream home use | 🟡 |
| "Mimiclaw: OpenClaw-like for ESP32" | CNX Software (Feb 13) | Analyse déploiement microcontrôleurs, gateway Telegram-Claude, hardware control low-RAM | 🟢 |

### Community Context — Ecosystem Fragmentation

Feb 13 marks acceleration of OpenClaw ecosystem fragmentation:

**Three parallel tracks emerge:**
1. **OpenClaw mainstream** — v2026.2.12 security-focused, features accumulation
2. **PicoClaw lightweight** — 4.5K stars in 48h, $10 hardware, speed/RAM optimized
3. **Mimiclaw embedded** — ESP32-S3, IoT/edge AI, microcontroller-grade

**Strategic implications:**
- **User segmentation:** Cloud prod (OpenClaw) vs Edge/IoT (PicoClaw/Mimiclaw)
- **Complexity vs simplicity:** Community fork reaction to OpenClaw feature creep
- **LobsterOps positioning:** Multi-runtime expertise (cloud + edge) becomes differentiator

**Security landscape post-ShieldClaw audit:**
- 64% prod deployments without security layer = systemic risk
- $34K+ documented losses legitimize security-first approach
- ShieldClaw 0 breaches on 1,400+ agents = validation of layered defense

---

## Week of February 10-12, 2026


### Releases

**v2026.2.9** (Feb 9) — Current stable version. YouTube update published, full release notes TBD.

No new release since v2026.2.9 (Feb 9). 164 commits on main since release — next version likely imminent.

**GitHub stats (Feb 12):** 185K stars (+6K), 31K forks (+1.3K). Wikipedia article now live in French and English.

### @steipete — Creator Signals

| Date | Signal | Tag |
|------|--------|-----|
| **Feb 12** | **Podcast with Lex Fridman** — full-day conversation on OpenClaw's origin, viral growth, rebrands, security, coding with agents, future plans. 3+ hour episode. | 🟡 |
| Feb 11 | ClawHub "unusable" — agrees with community criticism, needs pause before addressing | 🔴 |
| Feb 11 | Investigating **Google banning accounts for OAuth with OpenClaw** — may remove from providers if confirmed | 🔴 |
| Feb 11 | Netlify phone number leak fixed as security incident in half a day | 🟢 |

### Security — Critical Escalation

| Date | Signal | Tag |
|------|--------|-----|
| **Feb 11** | **🔴 CVE-2026-25157 DISCLOSED** — SSH command injection in macOS app (CVSS 7.8). GitHub Advisory GHSA-g8p2-7wf7-98mq. Patch status unknown. | 🔴 ⬆️ |
| **Feb 9-11** | **Exposure escalation: 40K → 135K+ instances** — SecurityScorecard STRIKE report shows exponential growth during scan period. The Register confirms 135K+ internet-facing by end of report. | 🔴 ⬆️ |
| **Feb 11** | **15,200 instances vulnerable to remote takeover** — H2S Media reports precise vulnerable subset from 42,900 total exposed instances. | 🔴 |
| **Feb 9** | **12,812 instances still vulnerable to CVE-2026-25253** — STRIKE scan finds unpatched instances despite v2026.1.29 patch available since Jan 29. | 🔴 |
| **Feb 3-9** | **CVE-2026-25253 widespread coverage** — Major security outlets (TheHackerNews, SOCRadar, U of Toronto, CCB Belgium) publish advisories. 1-click RCE via WebSocket hijacking remains most critical vector. | 🔴 |
| **Feb 10** | **Kaspersky** — Official blog: "OpenClaw AI agent found unsafe for use" — synthesis of critical vulns, home user recommendations | 🔴 |
| **Feb 11** | **SOCRadar** — Deep dive on CVE-2026-25253, links to Patch Tuesday, broader AI framework risks | 🔴 |
| **Ongoing** | **Cyera Research Labs** — "The OpenClaw Security Saga" — best analysis on data governance + agents. Introduces **"Data Gravity" concept**: agent aggregates OAuth tokens, API keys, SaaS permissions → any compromise gains disproportionate reach | 🔴 |
| **Ongoing** | **SecurityScorecard STRIKE** (updated) — Numbers revised upward: **135K+ exposed**, **50K+ RCE-vulnerable**, **53K+ linked to prior breaches** | 🔴 ⬆️ |
| **Ongoing** | **Bitdefender** (updated) — Now reporting **800+ malicious skills** (up from ~400 in initial analysis), automated deployment scripts detected | 🔴 ⬆️ |
| **Feb 11** | **Google OAuth banning** — steipete investigating accounts banned for using OAuth with OpenClaw | 🔴 |

### Releases — Feature Updates

| Date | Version | Signal | Tag |
|------|---------|--------|-----|
| **Feb 7** | **v2026.2.6** | Support Anthropic Opus 4.6, OpenAI Codex GPT-5.3-Codex, xAI Grok provider. Token usage dashboard added to Web UI. **Safety Scanner** feature mentioned (CyberSecurityNews) — details TBD. | 🟡 📌 |

### Ecosystem & Community

| Date | Signal | Tag |
|------|--------|-----|
| **Feb 10** | **🟡 OpenClawd launches managed hosting platform** — Targets users who failed DIY setup. Free + premium tiers, one-click deployment, auto security/uptime management. Explicitly positions against self-hosting complexity (WhatsApp integration pain point cited). First commercial hosted competitor. | 🟡 ⬆️ |
| **Feb 12** | **Wikipedia EN article updated** — Full history (Clawdbot → Moltbot → OpenClaw) now documented. Article stable after rebranding period. | 🟢 |
| **Feb 9** | **YouTube major update** — Significant v2026.2.9 update announcement (Lilys AI summary). Content TBD. | 🟢 |
| **Feb 10** | **DigitalOcean publishes OpenClaw guide** — "What is OpenClaw?" resource + 1-Click Deploy ($24/mo hardened image) officially promoted. | 🟢 |
| **~Feb 5** | **xCloud publishes managed vs self-hosting comparison** — Community debate on hosted vs DIY intensifies pre-OpenClawd launch. | 🟢 |
| **Feb 11** | **OpenClaw as CRM assistant** — @manchatz (X) forwards emails, logs calls, tracks quotations. Enterprise use case | 🟢 |
| **Feb 11** | **Raspberry Pi 5 build** — @Melaniepelz_ (X) Ollama + OpenClaw on Pi 5 16GB. Works but speed-limited | 🟢 |
| **Feb 11** | **Agent auth = next startup opportunity** — @ryancarson (X) auth for deploy/logs/CI/CD/email/git/DB is the blocker | 🟡 |
| **Feb 11** | **IPv6 fix for Telegram** — @pben4ai (X) `NODE_OPTIONS` to force IPv4, fixes timeout on partial IPv6 ISPs | 🟢 |
| **Ongoing** | **1-click deploy service sold $225K** — openclaw.report 397 subscribers, $17K MRR. First major commercial exit in ecosystem | 🟡 |

### Community Context — Hosting Landscape

The OpenClawd launch (Feb 10) marks a critical inflection point: OpenClaw transitions from pure self-hosted project to commercial managed service competition. Multiple hosting tiers now exist:

- **Free DIY:** Oracle Cloud free tier ($0/mo)
- **Budget VPS:** Hetzner ($4/mo), self-managed
- **Managed entry:** OpenClawd (pricing TBD)
- **Premium managed:** DigitalOcean 1-Click ($24/mo)
- **Enterprise DIY:** Custom (LobsterOps positioning)

Security posture becomes key differentiator: 135K+ exposed instances demonstrate default-insecure self-hosting risks vs managed security-by-default promise.

### Industry — Wider AI Agent Ecosystem (Feb 10-12)

| Signal | Source | Tag |
|--------|--------|-----|
| **OpenAI Responses API: agentic primitives** | VentureBeat (Feb 10) — Server-side compaction (5M tokens, 150 tool calls), hosted shell containers (Debian 12 + networking), **Skills standard in API** (same SKILL.md as Anthropic/OpenClaw). Triple Whale: "Moby" agent runs multi-hour sessions | 🟡 |
| **GLM-5 open-source (744B, MIT)** | Z.ai (Feb 11) — New frontier open-source model. SWE-bench 77.8% (vs Opus 4.5: 80.9%). Compatible Claude Code + OpenClaw + OpenRouter. Generates .docx/.pdf/.xlsx natively | 🟡 |
| **Anthropic ASL-4 Sabotage Report** | Anthropic (Feb 11) — First public sabotage risk report for a production model (Opus 4.6). Precedent for safety transparency | 🟢 |
| **Kimi Agent Swarm: 100 sub-agents** | Moonshot AI (Feb 10) — Orchestration of 100 parallel sub-agents, 1500+ tool calls, 4.5x speedup. Resolves context degradation on long tasks | 🟡 |
| **OpenAI Harness Engineering** | OpenAI (Feb 11) — ~1M lines, ~1500 PRs, zero manual code with Codex. 3.5 PRs/engineer/day. Uses AGENTS.md as table of contents (same pattern as OpenClaw) | 🟢 |
| **SKILL.md standard convergence** | VentureBeat — OpenAI + Anthropic + OpenClaw converge on same SKILL.md (YAML frontmatter + markdown). Skills are now portable cross-platform. Glean: tool accuracy 73% → 85% with Skills framework | 🟡 |
| **Skilld** | @harlan-zw — Generates AI agent skills from npm dependencies. Versioned best practices, local-first. github.com/harlan-zw/skilld | 🟢 |
| **SkillRadar v1.1.0** | Community — Agent-driven skill search and recommendation. Searches, compares, recommends skills for you | 🟢 |

### Community Field Reports — Operational Patterns

| Pattern | Source | Key Insight |
|---------|--------|-------------|
| **Memory architecture split** | "3 weeks running 24/7" thread | Don't dump everything in MEMORY.md. Split: `active-tasks.md` (crash recovery), `YYYY-MM-DD.md` (daily logs), `projects.md`/`lessons.md`/`skills.md` (thematic). Agent loads only what it needs |
| **Crash recovery pattern** | Same thread | `active-tasks.md` as safety net — write on START, note sub-agent session keys, update on COMPLETE. On restart agent resumes autonomously |
| **Security model tiering** | Same thread | Opus for ANY task reading external content (web/emails/tweets = prompt injection risk). Sonnet for internal ops only |
| **HEARTBEAT.md < 20 lines** | Same thread | Keep heartbeat minimal — just checklist. Heavy work goes in cron jobs. Saves massive token burn |
| **Prescriptive memory** | "AI agent on OpenClaw" thread | End daily logs with "Next Actions" not "What We Discussed." Agent needs to know what to DO, not what happened |
| **Trust Tiers** | Same thread | Files/research = autonomous. Tweets/emails = approval queue. Money/credentials = off limits |
| **Daily Synthesis Loop** | Same thread | Evening: review → extract patterns → propose improvements → implement. Yesterday's insight = today's capability |
| **Skills routing logic** | "3 weeks 24/7" thread | Add "Use when / Don't use when" in each skill description. Without this, ~20% misfire rate |

### Key Articles & Resources

| Article | Source | Key Contribution | Tag |
|---------|--------|-----------------|-----|
| "The OpenClaw Security Saga" | Cyera Research Labs | Data governance + data gravity concept for agent security | 🔴 |
| "25 Ways to Deploy Your OpenClaw Agent" | ZHC Research / IZHC | Comprehensive deployment guide: 5 tiers, 25+ platforms. Oracle Cloud free tier, Claw Cloud, MissionControlAI, EasyClaw | 🟡 |
| "Software Enters the Autopilot Era with AI Agents" | Editorial (Feb 11) | Industry shift from copilot to autopilot — governance, SaaS economics, workforce implications | 🟢 |
| "The Zero Human Company Run By Just AI" | ReadMultiplex / Brian Roemmele | First documented ZHC attempt — Claude Code + Grok + 6TB local dataset, open-source workflow planned | 🟢 |
| "OpenClaw AI agent found unsafe for use" | Kaspersky | Mainstream security vendor synthesis — risks + practical recommendations | 🔴 |
| "AI Agents as RPG Characters" | VoXYZ (@VOXYZ_AI) | Most complete agent identity architecture: 6-layer Role Cards, Affinity Matrix (15 pairs + drift), memory-driven personality evolution, RPG stats as observability. Full code. → TD Annex E.8-E.11 | 🟡 📌 |
| "After Installing OpenClaw for 50 Teammates" (full version) | Team9.ai / Winrey | Enriched field report: "One-Click Install guides are a lie", ADHD founder context-switch cost, cloud-native solution "LEAVE ME ALONE!!!", post-migration 10x velocity. Now open-source | 🟡 📌 |

---

## Week of February 3-9, 2026

### Releases

| Version | Date | Highlights | Tag |
|---------|------|------------|-----|
| **v2026.2.9** | Feb 9 | iOS alpha node app + setup-code onboarding, Grok web_search provider, agent management RPC (create/update/delete via Web UI), device pairing plugins, massive Telegram hardening, OPENCLAW_HOME override, Windows path fix, bindings hot-reload, Voyage embeddings input_type, QMD shared model cache, .caf audio transcription | ðŸŸ¡ |
| **v2026.2.6** | Feb 7 | Opus 4.6 + GPT-5.3-Codex support, xAI Grok provider, token usage dashboard, Voyage AI native memory, sessions_history cap context overflow | ðŸŸ¡ |
| **v2026.2.3** | Feb 5 | Cloudflare AI Gateway, Moonshot onboarding, responsePrefix per-channel, cron delivery modes, Feishu plugin | ðŸŸ¡ |

**GitHub stats (Feb 10):** 179K stars, 29.7K forks

### Security â€” Critical Escalation

| Source | Signal | Tag |
|--------|--------|-----|
| **SecurityScorecard STRIKE** | 135K+ internet-exposed instances (exponential growth from 42K) | ðŸ”´ ðŸ“Œ |
| **Bitdefender** | ~900 malicious skills (~20% of ClawHub registry), 14 identified actors | ðŸ”´ ðŸ“Œ |
| **Zenity Labs** | Indirect prompt injection via Google Doc to C2 Sliver. Zero-click | ðŸ”´ ðŸ“Œ |
| **Snyk** | 283 skills (7.1%) expose credentials in plaintext. 1,467 skills (36%) with vulns | ðŸ”´ ðŸ“Œ |
| **Palo Alto Unit 42** | "Lethal Trifecta": vulnerable by design | ðŸ”´ ðŸ“Œ |
| **Giskard** | Cross-workspace leakage between DM/group sessions | ðŸ”´ ðŸ“Œ |
| **BitsecAI** | First holistic audit (400K+ LOC), 100+ vulns, VULN-188, VULN-210 | ðŸ”´ ðŸ“Œ |
| **Android Authority** | ZeroLeaks score 2/100 | ðŸŸ¡ ðŸ“Œ |
| **Chinese Ministry of Industry** | Official warning about misconfiguration risks | ðŸŸ¡ ðŸ“Œ |

**New security tools:** ClawSec (Prompt Security), Bitdefender AI Skills Checker

### Ecosystem & Community

| Signal | Source | Tag |
|--------|--------|-----|
| **Kimi-K2.5 = #1 model on OpenClaw** (via OpenRouter) | @KimiProduct | ðŸŸ¡ ðŸ“Œ |
| **Opus 4.6 token hunger**: "basically unusable" for planning | Discord IZHC | ðŸŸ¡ ðŸ“Œ |
| **ClawRouter**: 30+ models, ~70% savings, 400 stars in 48h | BlockRunAI | ðŸŸ¡ ðŸ“Œ |
| **Claw Compactor**: transcripts 97% smaller | @Nielsen777Brian | ðŸŸ¢ ðŸ“Œ |
| **Team9.ai**: AI OS for teams, 50 teammates | Winrey | ðŸŸ¡ ðŸ“Œ |
| **Collective Intelligence** as next multi-agent bottleneck | @JUMPERZ, @Spark_coded | ðŸŸ¡ ðŸ“Œ |

### Key Articles

| Article | Source | Key Contribution | Tag |
|---------|--------|-----------------|-----|
| "My Marketing Co-Founder Is an AI Agent" | ScreenSnap Pro | 8-agent blueprint, $0.70/article | ðŸŸ¡ ðŸ“Œ |
| "After Installing OpenClaw for 50 Teammates" | Team9.ai | Context Rot, Plugin Problem, Integration Tax | ðŸŸ¡ ðŸ“Œ |
| "The Living Files Theory" | scalesoftware.ai | Dead vs living files, verbalization problem | ðŸŸ¢ ðŸ“Œ |
| "YC hosted Boris, creator of Claude Code" | @bcherny | 100-line agentic loop, zero-based prompt budgeting | ðŸŸ¡ ðŸ“Œ |

### Field Reports â€” Discord IZHC (Feb 5-10)

- **Composio** for auth (20K free calls/month), **Typefully MCP** for social posting
- **ap's tiering**: Codex 5.3 for everything except tweet drafts/PRDs (force Opus)
- **Budget pitfall**: $150 burned in one Opus 4.5 planning session
- **VPS security**: Pointbreak pattern = pragmatic consensus

---

## Watch Backlog

- [x] **Skill Graphs > SKILL.md (Heinrich/arscontexta)** — ✅ Documenté (v1.4, Feb 18). Pattern graphe de knowledge interconnecté, plugin 249 fichiers, pertinence LobsterOps base 150KB+



- [x] **Peter Steinberger → OpenAI** — ✅ Documenté (v1.3, Feb 17). Foundation MIT, emploi pas acquisition, Meta rejeté
- [x] **v2026.2.15 nested sub-agents + LLM hooks** — ✅ Documenté (v1.3, Feb 17)
- [x] **Kimi Claw launch (Moonshot AI)** — ✅ Documenté (v1.3, Feb 17)
- [ ] **Base Ecosystem ($1M/mois incentive program)** — Mentionné par Grok (ClawIndex, Bankr CLI, BOTCOIN, LLM gateway self-pay, incentive jusqu'à $1M/mois). **Non confirmé par sources web indexées** (Forbes général, TechFlow Base AI Season non extractible, aucune mention des outils cités). Pattern identique ShieldClaw. En attente vérification thread X ou annonce officielle Base
- [ ] **ShieldClaw** — Outil sécurité mentionné par Grok (stats 64% prod sans sécurité, 16% compromis, $34K pertes, 1,400+ agents protégés). **Stats non confirmées par sources web publiques** — en attente vérification (thread X ou GitHub). Si trouvé, réévaluer pour intégration
- [ ] **Reddit thread "farmer runtime defense"** — DIY security patterns communauté, signal intéressant pour patterns émergents
- [ ] Lex Fridman podcast — deep analysis when full episode available
- [ ] Google OAuth banning — monitor steipete investigation outcome
- [ ] ClawHub overhaul — steipete acknowledged "unusable", expect changes
- [ ] Collective Intelligence patterns (Spark: 166 agents)
- [ ] Clawathon results
- [ ] ClawRouter USDC wallet security audit
- [ ] "Basic to production" guide (community gap)
- [ ] Opus 4.6 vs 4.5 token benchmark
- [ ] Cyera "Data Gravity" concept — deeper analysis for Deep Dives
- [ ] ZHC deployment guide — evaluate new platforms (Claw Cloud, EasyClaw, MissionControlAI)

---

## Historical Releases

| Version | Date | Highlights |
|---------|------|------------|
| v2026.2.2 | Feb 4 | QMD backend, safety guardrails |
| v2026.1.29 | Jan 30 | **Fix CVE-2026-25253 (CRITICAL)** |
| v2026.1.25 | Jan 26 | Security adjustments |
| v2025.12 | Dec 2025 | Previous major version |

---

*OpenClaw Ecosystem Watch â€” Community Edition v1.0. February 2026.*
