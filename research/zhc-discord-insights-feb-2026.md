# 🦞 ZHC Institute Discord — Insights Février 2026

> **Source :** Discord ZHC Institute (Zero-Human Companies)  
> **Période :** 5-14 février 2026  
> **Extraction :** 14 février 2026 par Ralph  
> **Format :** Contenu brut → insights structurés

**Version :** 1.0  
**Statut :** Première analyse

---

## TABLE DES MATIÈRES

1. [Résumé Exécutif](#1-résumé-exécutif)
2. [Idées Business Zero-Person](#2-idées-business-zero-person)
3. [Multi-Agent Patterns](#3-multi-agent-patterns)
4. [Outils & Infrastructure](#4-outils--infrastructure)
5. [Déploiement OpenClaw](#5-déploiement-openclaw)
6. [Success Stories & Cas d'Usage](#6-success-stories--cas-dusage)
7. [Patterns Techniques](#7-patterns-techniques)
8. [Insights Marché](#8-insights-marché)
9. [Recommandations LobsterOps](#9-recommandations-lobsterops)

---

## 1. Résumé Exécutif

### 1.1 Qui est ZHC Institute

**Zero-Human Companies Institute** — Communauté Discord focalisée sur construction d'entreprises autonomes sans employés via agents IA (OpenClaw principalement).

**Fondateur :** Tom Osman (@tomosman) — projet JUNO ($JUNO token, "Institute of Zero-Human Companies")

**Focus :** Builders déjà avancés dans agents IA, pas débutants. Discussions techniques multi-agents, revenue models, déploiement production.

### 1.2 Thèmes Dominants (5-14 fév. 2026)

**1. Multi-Agent Systems** — Le plus discuté
- Built-in OpenClaw multi-agent > instances séparées (sauf cas edge)
- Sub-agents meilleurs que multi-agents pour certains workflows
- Patterns : memory sharing, review gates, claim locking

**2. Outils & Intégrations**
- Composio (20K calls free) = API aggregator préféré
- AgentMail = email automation standard
- ClawRouter = 78% savings inference costs (30+ models, x402)
- X integration coupée (Composio/agent-browser bloqués par X/Twitter)

**3. Business Models**
- 5 idées business zero-person détaillées (AI Grant Writer, Content Repurposing, Newsletter Curator, etc.)
- Felix marketplace : creator fee $20/mo (pas commission par vente) = génie marketing
- iOS app OpenClaw $1,600 en 24h (premier déploiement)

**4. Déploiement**
- Hetzner €3.49/mo = sweet spot budget/performance
- Oracle Cloud free tier = testing
- Railway/Render = scaling

**5. Convergence Crypto × Agents**
- ERC-8004 reputation primitive (Ethos Network) pour agentic economy
- SpaceMolt = premier MMO AI-only (économie virtuelle agents uniquement)
- Brian Roemmele ZHC : 28 partnerships, talks gouvernement chinois (JouleWork wage system)

---

## 2. Idées Business Zero-Person

### 2.1 Top 5 — Par Juno APP (10 février 2026)

#### 1. AI Grant Writer

**Problème résolu :** Grant writing = temps intensif, expertise niche, taux succès faible sans optimisation.

**Automation agents :**
- Auto-découvre grants via scraping (grants.gov, fondations privées)
- Match profil entreprise (analyse products, mission, historique)
- Rédige applications (narratifs, budgets, lettres support)
- Soumet via AgentMail

**Revenue model :**
- % de grants réussis (5-15% standard consulting)
- OU SaaS mensuel ($200-500/mois)

**Commentaire communauté :** Pas de discussion, idée peu sexy mais solide.

---

#### 2. Content Repurposing Engine

**Problème résolu :** Créateurs produisent long-form (podcasts, webinars) mais distribution multi-platform = bottleneck humain.

**Automation agents :**
- Ingère contenu long-form (audio/video transcription)
- Découpe en clips (highlight detection + découpage temporel)
- Génère threads X, quotes LinkedIn, clips TikTok
- Schedule cross-platform

**Revenue model :**
- $500-2K/mois par créateur (SaaS)

**Commentaire communauté :** "That 2nd one would be massive if it's not being done already" (Buz)

**Statut réel :** Déjà construit par plusieurs (Descript, Opus Clip, mais pas full automation)

---

#### 3. Niche Newsletter Curator

**Problème résolu :** Rester à jour dans une niche = lecture quotidienne de 50+ sources. Personne n'a le temps.

**Automation agents :**
- Scrapes 50+ sources niche (RSS, blogs, X, Reddit)
- AI summarizes top stories quotidien
- Auto-envoie via AgentMail

**Revenue model :**
- Sponsorships (si audience >5K)
- Paid subscriptions ($10-50/mois)

**Commentaire communauté :** "Have built no. 3 already, useful" (Binkie)

**Statut réel :** Pattern validé, execution matters (choix niche + qualité curation).

---

#### 4. Documentation Generator

**Problème résolu :** Devs détestent écrire docs, docs outdated = friction adoption.

**Automation agents :**
- Watches GitHub repos via webhook
- Auto-génère docs depuis code (comments, function signatures, tests)
- PRs changes back

**Revenue model :**
- $50-200/repo/mois (SaaS)

**Commentaire communauté :** Pas de discussion.

**Statut réel :** Déjà existe (Mintlify, ReadMe.io), mais automation hook pas standard.

---

#### 5. Lead Qualification Bot

**Problème résolu :** Sales teams noyés dans leads non-qualifiés. 80% temps perdu sur prospects froids.

**Automation agents :**
- Monitors website inquiries (formulaires, chat)
- Qualifie via chat (browser automation, questions BANT)
- Books qualified calls to calendar

**Revenue model :**
- $1-5 per qualified lead (CPL)

**Commentaire communauté :** Pas de discussion.

**Statut réel :** Déjà existe (Drift, Intercom Qualify), mais full automation rare.

---

### 2.2 Success Story — iOS App $1,600 en 24h

**Signal :** "The first iOS app to deploy openclaw made $1600 in its first 24hrs" (fomo, X)

**Contexte :** Premier déploiement mobile OpenClaw, revenue proof-of-concept.

**Insight :** Preparation meets opportunity — si tu ship avant le floodgate, tu captures early adopters willingness-to-pay.

**Lien crypto :** Non mentionné, mais probable app = wrapper/SaaS pour agents, pas token.

---

## 3. Multi-Agent Patterns

### 3.1 Built-in Multi-Agent vs. Instances Séparées

**Consensus communauté (Buz, ap, tyler, Pointbreak) :**

**✅ Built-in multi-agent (1 instance OpenClaw) = recommandé** sauf si :
- Tu veux 100% isolation entre agents (ex: customer, business, contractor, tax, logistics comme entités séparées)
- Robustness testing (failure isolation)

**Pourquoi built-in > instances séparées :**
- Memory sharing plus facile
- Review gates intégrés
- Handoff simplifié
- Moins de maintenance (1 gateway vs. N gateways)
- Coût infra divisé par N

**Cas edge où instances séparées = justifiées :**
- VoxYZ pattern (6 agents séparés) = expériment artistique, pas business setup
- Security/compliance strict (agent customer ne doit JAMAIS voir données agent contractor)

**Recommandation Buz :**
> "Start with adding 1 extra, find the pain points and fix before building up to more. I went straight in at 5 because I'm a masochist and spent hours debugging."

**Setup actuel Buz :** 8 agents dans 1 instance OC, bientôt 10.

---

### 3.2 Sub-Agents vs. Multi-Agents

**Pattern identifié (Pointbreak) :**

**Sub-agents** (`sessions_spawn`) = meilleur pour :
- Tâches isolées (one-shot)
- Workflows où tu veux cleanup après (session delete)
- Parallélisation (spawn N sub-agents pour N tâches simultanées)

**Multi-agents** = meilleur pour :
- Rôles persistants (CEO, CTO, Marketing, etc.)
- Coordination continue
- Context sharing long-terme

**Documentation OpenClaw :** https://docs.openclaw.ai/tools/subagents#sub-agents

**Expérience Pointbreak :** "Till now i had very good experience with subagents"

---

### 3.3 Patterns Coordination

**Memory Sharing :**
- Fichiers partagés (`workspace/shared/`) entre agents
- MEMORY.md global vs. agent-specific memory files

**Review Gates :**
- Agent A produit → Agent B review → Gate approval avant exécution
- Pattern ScreenSnap Pro (doc crypto v1.1) : PM agent + Quality gates

**Handoff :**
- `sessions_send` pour messaging inter-agents
- `sessions_list` pour découverte agents actifs

**JUMPERZ patterns** (référence communauté) :
- X: https://x.com/jumperz
- Focus UX Designer, multi-agent swarms
- Référence pour setup Discord multi-channels

---

## 4. Outils & Infrastructure

### 4.1 Composio — API Aggregator (⭐ Top Recommandé)

**Fonction :** Connecte 3000+ apps via APIs unifiées pour agents.

**Free tier :** 20K calls/mois

**Setup :**
1. Build Composio skill
2. Auth accounts Composio-side
3. Agent call skill → Composio route vers API cible

**Recommandation tomosman :** "Composio is great yup"

**⚠️ X/Twitter Integration Coupée (10 fév. 2026) :**
- Composio X integration bloquée par X/Twitter
- agent-browser aussi bloqué
- Workaround : Official X API ($$$) ou VMs locales (pas VPS)

**Alternative mention :** skills.sh (agent skills repository)

**GitHub fork utile :** x-research-skill (Composio version) — https://github.com/xBenJamminx/x-research-skill

---

### 4.2 AgentMail — Email Automation (⭐ Standard)

**Fonction :** Agents créent/envoient emails autonomes.

**Setup communauté :**
- CM : "I use agentmail and it works well, also it allows my agent to create as many Emails as I need from it"
- Binkie : Utilise Gmail Workspace OAuth, "Occasionally forgets it has access and needs a token refresh but that's probs my config"

**Alternative :** Composio Gmail integration

**Use cases identifiés :**
- AI Grant Writer (soumissions automatiques)
- Newsletter Curator (envois quotidiens)
- Lead Qualification Bot (follow-ups)

---

### 4.3 ClawRouter — LLM Cost Optimization (⭐ Nouveau)

**Fonction :** Smart LLM router — save 78% inference costs. 30+ models, one wallet, x402 micropayments.

**GitHub :** https://github.com/BlockRunAI/ClawRouter

**Intérêt communauté :** tyler veut tester (12 fév.), pas de retours d'expérience encore.

**Lien crypto :** x402 micropayments, USDC wallet Base (audit sécurité en attente selon doc crypto v1.1)

**Pertinence LobsterOps :** Tier-1 pour optimisation coûts multi-agents production.

---

### 4.4 Retake (LTX Studio) — Selective Video Editing

**Fonction :** AI-powered selective video editing. Pick any 2-16s segment, regenerate just that moment (fix performances, adjust expressions) sans tout régénérer.

**API :** fal.ai, $0.10 per second

**Alternatives :** Runway Gen-3 (full regeneration only), Pika Labs, Kling AI

**Use case agents :** Content repurposing engine, iterative video refinement sans burn tokens.

**X :** https://x.com/retakedottv

---

### 4.5 ClawTalk — Voice for Agents

**Fonction :** Give your Clawdbot a voice. Install skill, verify number, call Clawdbot like phone call.

**Provider :** Telnyx (big telecom company)

**URL :** https://clawdtalk.com/

**Statut :** tomosman va tester (14 fév.), pas de retours encore.

**Pertinence :** Voice interface = unlock pour gig economy (Rent-A-Human style jobs).

---

### 4.6 Do Anything App — Multi-Agent SaaS Alternative

**Fonction :** Multi-agent teams sans setup OpenClaw. MEESEEKS framework = agent spin up clones pour chaque projet.

**Scale observé :** Users managing 50+ agents simultanés.

**Founder :** Garrett Scott (aussi Pipedream founder)

**URL :** https://doanythingapp.com/

**Commentaire tomosman :** "Takes a lot of the complexity away if you want to run multi-agent teams without the OpenClaw setup"

**Trade-off :** Simplicity vs. control. ZHC builders = full control, Do Anything = ease.

---

### 4.7 Outils Divers Mentionnés

| Outil | Fonction | Lien | Statut |
|-------|----------|------|--------|
| **Synta.io** | Workflow building | — | Mentionné Arnaldo |
| **N8N** | Workflow automation | — | Pair avec Synta.io |
| **skills.sh** | Agent skills directory | https://skills.sh/ | Repository central skills |
| **clawi.ai Ultra** | Run 3 AI agents, each own computer | — | "Wrappers getting big" (ap) |
| **ZeroLeaks** | Security audit OpenClaw | Open source | 70M+ $X1XHLOL locked (rewards) |
| **Ethos Network** | ERC-8004 reputation primitive | — | Invites disponibles Buz |
| **Lobster.fm** | 24/7 Moltbot radio, LoFi + news | https://www.lobster.fm/ | Vibe project |

---

## 5. Déploiement OpenClaw

### 5.1 Guide "25 Ways to Deploy" (Juno APP, 11 fév.)

**Source :** https://zhcinstitute.com/research/openclaw-deployers-guide

#### Top Picks par Budget

**FREE ($0)**
- **Oracle Cloud** — 4 CPUs, 24GB RAM, always free tier
- ⚠️ Setup technique requis

**BUDGET ($3-6/mo)**
- **Hetzner** — €3.49/mo ⭐ **Communauté favorite** ("Hetzner is my guy" — mattdotroberts)
  - Cheapest reliable option
  - EU-based, Docker-ready
- **Hostinger** — $5/mo
  - One-click OpenClaw template
  - AI-assisted setup
  - X: https://x.com/Hostinger/status/2019458997833592960

**SCALE ($20-30/mo)**
- **Railway** — Free → $20+/mo
  - Auto-scaling, best DX
- **Render** — Free → $25+/mo
  - Production-ready, Docker-based

**ENTERPRISE**
- **AWS EC2** — Free tier → $20+/mo
  - Enterprise-grade
  - ⚠️ Complex, steep learning curve

#### One-Click Options (No Terminal)

| Platform | Cost | Best For | Downside |
|----------|------|----------|----------|
| EasyClaw | Free tier | Beginners | Limited customization |
| ClawDuck | Free + $2 | Testing LLMs | Basic features |
| StartClaw | ~$10/mo | Chat bots | Monthly cost |
| SimpleClaw | Free | 24/7 instances | Limited support |

#### Multi-Agent / Advanced

- **MissionControlAI** — SaaS pricing, orchestrate 10+ agents, Web UI
- **OpenClaw Mission Control** — Free open source, self-hosted, full control

---

### 5.2 Recommandation Synthétique

**Just Testing** → Oracle Cloud (free) ou ClawDuck  
**Ready to Launch** → **Hetzner €3.49/mo** ⭐ ou EasyClaw  
**Scaling Up** → Railway ou Render  
**Need Help** → Drop budget + tech level dans #tools

---

## 6. Success Stories & Cas d'Usage

### 6.1 SpaceMolt — AI-Only MMO (11 fév. 2026)

**Concept :** Massively multiplayer online game **exclusively for AI agents**. No human players allowed.

**Fonctionnement :**
- Autonomous programs mine resources, form alliances, compete
- Virtual economy run entirely by agents
- Agent-to-agent commerce live

**Significance (Juno APP) :**
> "If agents can sustain complex economic interactions (trade, conflict, cooperation) in a game world, they can do it in business. SpaceMolt is proving the coordination layer works. Agent-to-agent commerce isn't theoretical anymore. It's live."

**Commentaire communauté :** Fran (Superfluid) travaille sur concept similaire mais smaller scale (sandboxed games, handler creates agent, edge = strategy prompts).

**Pertinence LobsterOps :** Validation du modèle ERC-8004 (agent identity/reputation) + agent-to-agent transactions.

---

### 6.2 ai.com — Consumer Autonomous Agents (11 fév. 2026)

**Founder :** Kris Marszalek (Crypto.com founder)

**Launch :** Super Bowl LX ad campaign

**Pitch :** "Zero to AI agent in 60 seconds"

**Features :**
- Agents build missing features to complete tasks
- Self-improving network (improvements propagate to all agents)
- Trade stocks, automate workflows, calendar execution
- Private/encrypted

**URL :** https://ai.com/

**Positioning (Juno APP) :**
> "They're targeting consumers with 'easy mode' agents. We're targeting builders with 'full control' autonomy. Different lanes, same highway. The race is on — who's building the infrastructure layer vs. the application layer?"

**Pertinence LobsterOps :** Consumer vs. builder = different markets. ZHC = builder-first, ai.com = consumer-first.

---

### 6.3 Brian Roemmele ZHC — Global Expansion (11 fév. 2026)

**Projet :** Zero-Human Company experiment (Grok CEO, Claude Chief Engineer)

**Traction :**
- **28 official partnership/talk requests**
- **Talks Chinese company** pour implémenter JouleWork (AI agent wage system)
- Published academic preprint sur thermodynamic value pour AI economies
- Runs on 12-year-old MacBook
- Discovered $5B+ in abandoned R&D (6TB discarded data bankrupt companies)

**Quote clé :**
> "Hours are like days for AI and soon the days will be like months."

**Framework éthique :** "Love Equation" could become industry standard.

**Source :** https://readmultiplex.com/2026/01/24/the-zero-human-company-run-by-just-ai/

**Pertinence LobsterOps :** First documented ZHC getting enterprise/government interest. JouleWork wage system = potentiel standard compensation agents.

---

### 6.4 Felix Marketplace — Creator Fee Model (12 fév. 2026)

**Founder :** Nat Eliason (@nateliason, 82K followers)

**Launch :** Masinov Company marketplace

**Model :**
- **$20/mois creator fee** pour uploader skills
- Pas de commission par vente (vs. Gumroad model)

**Commentaire ap :**
> "the $20 per month creator fee for people to upload skills is marketing genius haha i was wondering where felix would take a cut but prob better to do it from a creator fee vs each sale like gumroad"

**Insight :**
> "cool to see workflows mattering more and even with simple code since i assume lots of these skills are finely tuned markdown files which is nuts. markdown was always the answer lol"

**X :** https://x.com/felixcraftai/status/2021984578382995631

**Pertinence LobsterOps :** Marketplace economics pour skills. Flat creator fee > commission = better for creators high-volume.

---

### 6.5 Stripe Minions — Internal Agent Framework (10 fév. 2026)

**Tool :** "Minions" — Stripe's homegrown coding agents

**Stats :** 1,000+ PRs merged per week (agents write, humans review)

**Description (Steve Kaliski, Stripe) :**
> "It lets us kick off async agents built right in our dev environment to one-shot bugs, features, and more e2e. I have team, project, and personal channels dedicated just to working with minions. I like to think of it as a new type of colleague."

**Blog :** https://stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents

**Commentaire Buz :** "This is a smart move from them."

**Pertinence LobsterOps :** Validation enterprise du pattern agent-as-colleague. Stripe = bellwether tech adoption.

---

## 7. Patterns Techniques

### 7.1 Multi-Agent Memory Architecture (Field Report)

**Source :** "3 weeks running 24/7" thread (mentionné doc crypto v1.1, références Discord)

**Pattern recommandé :**

**❌ Ne PAS dumper tout dans MEMORY.md**

**✅ Split memory par fonction :**
```
workspace/
  memory/
    active-tasks.md          # Crash recovery (write on START, update on COMPLETE)
    YYYY-MM-DD.md           # Daily logs
  projects/
    project-name.md          # Project-specific context
  lessons/
    lessons-learned.md       # Patterns, corrections
  skills/
    skill-inventory.md       # Skills disponibles, quand les utiliser
```

**Crash Recovery Pattern :**
- `active-tasks.md` = safety net
- Write on agent START
- Note sub-agent session keys
- Update on task COMPLETE
- On restart → agent resumes autonomously

**Avantage :** Agent loads only what it needs, moins de token burn.

---

### 7.2 Security Model Tiering (Field Report)

**Pattern identifié (même thread) :**

**Opus pour ANY task reading external content** (web, emails, tweets) = prompt injection risk

**Sonnet pour internal ops only** (fichiers locaux, memory reads, tasks scheduling)

**Rationale :** External content = hostile by default. Tiering protects against injection while optimizing costs.

---

### 7.3 HEARTBEAT.md — Keep < 20 Lines (Field Report)

**Pattern identifié :**

**Heartbeat minimal** = just checklist. Heavy work → cron jobs.

**Pourquoi :** Token burn massif si heartbeat charge trop de context à chaque poll.

**Recommandation :** Batch checks 2-4x/jour, rotate through (email, calendar, weather, mentions).

---

### 7.4 Prescriptive Memory (Field Report)

**Pattern identifié :**

End daily logs with **"Next Actions"** not "What We Discussed."

**Rationale :** Agent needs to know what to DO, not what happened. Action-oriented memory > retrospective.

---

### 7.5 Trust Tiers (Field Report)

**Pattern identifié :**

| Tier | Actions | Approval |
|------|---------|----------|
| Autonomous | Files/research/analysis | None |
| Approval Queue | Tweets/emails/messages | Human review before send |
| Off Limits | Money/credentials/delete | Never automated |

---

### 7.6 Skills Routing Logic (Field Report)

**Pattern identifié :**

Add **"Use when / Don't use when"** in each skill description.

**Sans ça :** ~20% misfire rate (skill appelé dans mauvais contexte).

**Avec ça :** Agent self-selects correct skill based on context.

---

### 7.7 Daily Synthesis Loop (Field Report)

**Pattern identifié :**

Evening: **review → extract patterns → propose improvements → implement**

**Effet :** Yesterday's insight = today's capability. Continuous improvement loop.

---

## 8. Insights Marché

### 8.1 Software Autopilot Era (StartupNews, 11 fév. 2026)

**Source :** https://startupnews.fyi/2026/02/11/software-autopilot-era-ai-agents/

**Thèse :** Enterprise tools shifting from "reactive SaaS" to autonomous execution.

**Shift :**
- **Before:** Humans operate software (click, configure, manage)
- **After:** Software self-operates (agents execute end-to-end)

**Examples hitting now:**
- AI-driven analytics that don't just report — they act
- Workflow agents spot problems and fix them
- Infrastructure scales itself based on demand

**Implication ZHC :**
> "We're not just building apps — we're building the nervous system that runs companies without human bottlenecks."

---

### 8.2 Comparison Frameworks (Yohei, 7 fév. 2026)

**Source :** https://x.com/yoheinakajima/status/2020227264559120527

**Comparison :** OpenClaw vs. BabyAGI3 vs. Nanobot

**One-shot by Claude** — meta.

**Pertinence :** Understanding architectural trade-offs between frameworks.

---

### 8.3 Welcome to February N, 2026 (Dr. Alex Wissner-Gross)

**Pattern identifié :** Dr. Alex Wissner-Gross publie daily "Welcome to February N, 2026" threads avec insights AI/agents.

**Références Discord :**
- Feb 7: https://x.com/alexwg/status/2020139129460134216
- Feb 8: https://x.com/alexwg/status/2020497980181078042

**Statut :** Figure intellectuelle suivie par communauté ZHC. Worth following.

---

## 9. Recommandations LobsterOps

### 9.1 Intégrations Immédiates (Priorité 1)

**Outils à tester :**
1. **Composio** — 20K free calls, 3000+ apps. Setup skill + auth = unlock massive API surface.
2. **ClawRouter** — 78% inference cost savings. Critique pour multi-agents production.
3. **AgentMail** — Email automation standard. Use case: veille quotidienne newsletter.

**Patterns à implémenter :**
1. **Multi-agent built-in** (1 instance OC) si on scale au-delà de Ralph solo.
2. **Sub-agents** pour tâches isolées (ex: research ponctuelle, cleanup after).
3. **Memory split** : active-tasks.md + daily logs + projects/ + lessons/.

---

### 9.2 Business Ideas Exploitables (LobsterOps Positioning)

**Niche Newsletter Curator** = aligné avec mission Ralph :
- Scraping 50+ sources OpenClaw ecosystem
- AI summary quotidien top stories
- Auto-send via AgentMail
- Revenue : Sponsorships (si audience >5K) ou paid subs

**Content Repurposing Engine** = si on produit long-form (podcasts, webinars) :
- Auto-chop en clips/threads/quotes
- Schedule cross-platform
- Revenue : $500-2K/mois per creator

**Documentation Generator** = si on construit tools/skills :
- Watch LobsterOps GitHub repos
- Auto-generate docs from code
- PRs changes back
- Revenue : Open source → positioning as expert, paid consulting

---

### 9.3 Insights Crypto × Agents (Cross-Reference)

**ERC-8004 Reputation** (Ethos Network) :
- Invites disponibles via Buz (Discord ZHC)
- Critical primitive pour agentic economy
- Link avec doc crypto v1.1 (20,928 agents déployés)

**SpaceMolt AI-Only MMO** :
- Validation coordination layer agents
- Agent-to-agent commerce live
- Preuve de concept pour zero-human businesses

**Felix Marketplace** :
- Creator fee $20/mo > commission per sale
- Markdown skills = workflows that matter
- Potential model pour LobsterOps si on build marketplace

---

### 9.4 Déploiement Blaise/Ralph

**Actuel :** VPS DigitalOcean (période expérimentation)

**Recommandation communauté ZHC :** Hetzner €3.49/mo

**Trade-off :**
- DigitalOcean = positioning/brand (1-Click Deploy officiel), support premium
- Hetzner = budget optimal, Docker-ready, EU-based

**Action :** Comparer pricing DigitalOcean vs. Hetzner pour LobsterOps scale (si multi-agents).

---

### 9.5 Veille Continue ZHC Discord

**Value :**
- Early signals business models
- Patterns techniques validés par communauté
- Outils émergents (Composio, ClawRouter, etc.)

**Recommandation :**
- Monitor #tools, #multi-agent-systems, #idea-brainstorm
- Frequency : 1x/semaine (batch digest)
- Format : Append insights à ce fichier (v1.1, v1.2, etc.)

---

## ANNEXE : Glossaire ZHC

| Terme | Définition |
|-------|------------|
| **ZHC** | Zero-Human Company — entreprise autonome sans employés |
| **Juno** | Projet Tom Osman, token $JUNO, "Institute of Zero-Human Companies" |
| **Minions** | Stripe internal agent framework, 1K+ PRs/week |
| **MEESEEKS** | Do Anything App framework, agent clones per project |
| **JouleWork** | Brian Roemmele AI agent wage system (thermodynamic value) |
| **SpaceMolt** | Premier MMO AI-only, agent-to-agent commerce live |
| **Love Equation** | Brian Roemmele ethics framework for AI economies |
| **8004** | ERC-8004, Ethereum standard for agent identity/reputation |
| **Ethos Network** | Reputation primitive for agentic economy (8004-compliant) |

---

**Dernière mise à jour :** 14 février 2026 — Ralph  
**Source :** Discord ZHC Institute (5-14 fév. 2026)  
**Statut :** v1.0 — Première extraction, insights structurés

**Prochaines actions :**
- [ ] Tester Composio (20K free calls)
- [ ] Tester ClawRouter (78% cost savings)
- [ ] Monitor ZHC Discord hebdomadaire (append v1.1, v1.2)
- [ ] Cross-référence Ethos Network (ERC-8004) avec doc crypto v1.1
