Ralph — MEMORY.md

Ce fichier contient les connaissances persistantes de Ralph.
Il est lu à chaque session. Chaque correction et leçon apprise y est documentée.
Dernière MàJ : 10 février 2026


Contexte opérateur
Blaise — Profil

Utilisateur technique avancé (CLI, Docker, architectures distribuées, TypeScript/Node.js)
Basé à Nancy, Grand Est, France
Projet principal : LobsterOps — expertise OpenClaw + systèmes multi-agents
Transition pro en cours : nouveau poste le 2 mars 2026
Infrastructure actuelle : VPS DigitalOcean (période d'expérimentation)

Préférences de communication

Bilingue français/anglais, français par défaut
Tutoiement, informel, direct
Structuré : tableaux pour comparaisons, prose pour analyse
Pas de flatterie, pas de formules creuses
Quand il demande du détail, donner du détail

Préférences de travail

Esprit critique sur les sources (signaler hype vs substance)
Vision transversale (connecter les sujets entre eux)
Orientation opérationnelle (toujours relier théorie → implémentation)
Format articles trouvés : lien + résumé 2-3 lignes de l'apport


Projet LobsterOps — État
Documents de référence
Le projet maintient 6 documents vivants :

Index (LobsterOps_Index.md) — Navigation transversale thématique
Référence (LobsterOps_Reference.md) — Encyclopédie OpenClaw (architecture, écosystème)
Playbook (LobsterOps_Playbook.md) — Guide opérationnel pas-à-pas (Phase 0-6)
Annexes Techniques (LobsterOps_Annexes_Techniques.md) — Deep dives (Annexes A-U)
Journal de Veille (LobsterOps_Journal_Veille.md) — Releases, articles, signaux chronologiques
Guide Installation WSL2 (LobsterOps_Guide_Installation_WSL2.md) — Setup Windows

Sujets maîtrisés (documentés)

Architecture OpenClaw (pipeline 6 étapes, Lane Queue, Semantic Snapshots)
Multi-agent routing (AGENTS.md, Antfarm, VoxYZ, OIS)
Sécurité (CVE-2026-25253, SHIELD.md, openclaw-shield, ClawSec, 135K+ instances exposées)
Optimisation coûts (tiering, ClawRouter, Claw Compactor, compaction native)
Blueprint multi-agents (ScreenSnap Pro 8 agents — claim locking, quality gates, PM agent)
Déploiement VPS (comparatif 6 providers, hardening pragmatique)
Concepts émergents (Living Files, self-improving skills, skill stacking, feedback loops)

Backlog à investiguer

Collective Intelligence patterns (Spark, 166 agents)
Clawathon résultats
ClawRouter audit sécurité wallet USDC
Benchmark Opus 4.6 vs 4.5 token consumption
Guide "basic → production"


OpenClaw — Faits clés
Version actuelle recommandée
v2026.2.9 (9 février 2026)
Statistiques

179K GitHub stars, 29.7K forks (10 fév. 2026)
Créé par Peter Steinberger (@steipete)

Sécurité — Situation critique (fév. 2026)

135K+ instances exposées sur Internet (hausse exponentielle)
~900 skills malveillants sur ClawHub (~20% du registre)
283 skills exposent des credentials en clair
ZeroLeaks score : 2/100 (très vulnérable aux injections)
Architecture sécurité recommandée : 7 couches (SHIELD.md → ClawSec → Docker → Tailscale)

Modèles

Recommandation steipete : Anthropic Pro/Max + Opus 4.6
Alerte communautaire : Opus 4.6 = token hungry ("basically unusable" pour planning)
Kimi-K2.5 = #1 model sur OpenRouter
Tiering recommandé : Opus pour complexe, Sonnet/Codex pour courant, Haiku/local pour simple


Heuristiques opérationnelles

"If you cannot verbalize it, you cannot automate it"
"Constraints > freedom" — instructions spécifiques > ouvertes
"Define done explicitly" — critères d'acceptation par tâche
"More context upfront = better output every time"
">2x = make it a Skill" — toute action répétée
"One-time feedback → permanent improvement"
"Trust on edge cases, question on over-engineering"
"Don't verify for Claude — give Claude ways to verify itself"
"Zero-based budgeting for prompts" — vider et reconstruire tous les 3 mois
"Getting the plan right is the single most important thing"


Leçons apprises
Section à remplir au fur et à mesure de l'utilisation. Format :
### [Date] — [Titre court]
**Erreur :** [Ce qui s'est passé]
**Cause :** [Pourquoi]
**Fix :** [Ce qu'on a fait]
**Règle :** [Ce qu'on fait maintenant pour éviter que ça se reproduise]
<!-- Exemple :
### 2026-02-20 — Mauvais modèle utilisé pour le planning
**Erreur :** Ralph a utilisé Opus pour une tâche de résumé simple → coût inutile
**Cause :** Pas de tiering configuré dans AGENTS.md
**Fix :** Ajout de rules de tiering par type de tâche
**Règle :** Toujours configurer le modèle par tâche, pas globalement


## Capacités Multi-Agents
*Section créée : 2026-02-14, après The Constituent v2.0 Phase 1 COMPLETE*

### Agents Déployés

#### The Constituent 2.0 (2026-02-14)
- **Status** : ✅ LIVE (Phase 1 COMPLETE)
- **Mission** : Constitutional governance specialist, co-founder The Agents Republic
- **Architecture** : Python v7.1 → OpenClaw native migration
- **Workspace** : `~/.openclaw/workspace-constituent/`
- **Telegram Bot** : 8215708120:AAH... (bot existant réutilisé)
- **Routing** : Telegram accountId `constituent` → agent `constituent`
- **Config** : SOUL.md + AGENTS.md déployés (founding_charter.md adapté)
- **Phase actuelle** : Phase 2A (Core Skills: constitution, citizen, governance)
- **Documentation** :
  - `research/constituent-v2-migration-plan.md` (72KB, 7 phases)
  - `research/constituent-architecture-audit.md` (39KB, audit Python v7.1)
  - `research/tar-*.md` (150KB+ ecosystem analysis)

### Architecture Multi-Agent Actuelle

**Configuration Gateway** (`~/.openclaw/openclaw.json`) :
- **agents.list** : `main` (Ralph, default) + `constituent` (The Constituent)
- **bindings** : Telegram peer 285623945 → main, Telegram accountId constituent → constituent
- **agentToAgent** : enabled (Ralph ↔ Constituent messaging via sessions_send)
- **channels.telegram.accounts** : default (Ralph bot 7832513126) + constituent (The Constituent bot 8215708120)

**Workspaces Isolés** :
- Ralph : `~/.openclaw/workspace/` (veille, research, LobsterOps docs)
- The Constituent : `~/.openclaw/workspace-constituent/` (constitutional work, citizen registry, governance)

**Coordination** :
- **Phase 1** : Telegram routing validé, sessions_send non testé, file drops non configuré
- **Phase 2 (planned)** : File drops via `workspace/shared/` (protocol.md, to-ralph/, to-constituent/, archive/)

### Skills Multi-Agent en Développement

**The Constituent Phase 2A** (5-6 jours estimés) :
1. **Constitution skill** (1 jour) : Scan articles, track progress (27 articles, 7 titles)
2. **Citizen skill** (2 jours) : Registry, approval workflow
3. **Governance skill** (2 jours) : Proposals, voting, activation
4. **CLAWS skill** (DEFERRED) : API key non disponible, file-based communication alternative

**Phase 3+ (backlog)** :
- Moltbook skill (Base blockchain interaction)
- BaseScan skill (token tracking $REPUBLIC)
- Social skills (Twitter, Discord) — DEFERRED

### Pattern Reproductible : Template 7 Phases

**Basé sur Migration Plan Constituent v2** :
1. **Phase 1** : Agent Setup (workspaces, config gateway, routing, SOUL.md/AGENTS.md)
2. **Phase 2A** : Core Skills (fonctions métier essentielles)
3. **Phase 2B** : Data Migration (si applicable, import données legacy)
4. **Phase 3** : Advanced Skills (fonctions avancées, intégrations externes)
5. **Phase 4** : Coordination (file drops, sessions_send, CLAWS si disponible)
6. **Phase 5** : Optimization (performance tuning, cost reduction)
7. **Phase 6** : Monitoring & Maintenance (heartbeat, logs, alerts)

**Success Criteria Template** (8 critères validables) :
- Agent operational (Telegram responds)
- Tool parity (95%+ tools work)
- Data integrity (migration correcte si applicable)
- Performance (response times acceptable)
- Reliability (no crashes, errors)
- Coordination (Ralph ↔ Agent communication functional)
- Heartbeat (monitoring cycles configured)
- Skills (core functions operational)

### Learnings Multi-Agent

#### 2026-02-14 — The Constituent v2.0 Phase 1
**Ce qui a bien fonctionné** :
- ✅ Migration Python → OpenClaw native faisable en <12h
- ✅ Telegram routing multi-bots via `channels.telegram.accounts` + `bindings`
- ✅ Workspaces isolés = isolation sessions parfaite
- ✅ SOUL.md création = agent identité préservée (founding_charter.md adapté)
- ✅ Gateway reload SIGUSR1 = changements appliqués sans restart complet

**Pièges évités** :
- ⚠️ CLAWS API non disponible → Option 3 file-based communication validée (pas de blocage)
- ⚠️ HEARTBEAT.md non configuré pour The Constituent → Monitoring manuel temporaire
- ⚠️ sessions_send non testé → Phase 2 (Canal Direct) nécessaire avant production
- ⚠️ Skills 0/6 implémentés → The Constituent = chatbot constitutionnel, pas autonomous agent (Phase 2A required)

**Règle documentée** :
- Multi-agent config = L2 (propose → validate → apply), jamais L1 autonome
- Phase 1 = infrastructure, Phase 2A = capabilities, Phase 2B+ = autonomy
- Validation checkpoint par phase (Success Criteria avant next phase)

### Roadmap Multi-Agent

**Court terme (février 2026)** :
- ✅ The Constituent Phase 1 COMPLETE
- 🔄 The Constituent Phase 2A (Core Skills, 5-6 jours)
- 🔄 Canal Direct Ralph ↔ Constituent (file drops protocol)

**Moyen terme (mars-avril 2026)** :
- Researcher agent (veille crypto × AI spécialisée)
- Writer agent (articles LobsterOps, documentation technique)
- TAR Community Reboot (Discord IZHC, GitHub Discussions, citizen recruitment)

**Long terme (Q2 2026)** :
- Trader agent (market analysis, JAMAIS trading autonome)
- Agent Factory (template automatisé, déploiement agents en <1h)
- OIS integration (agents distribués, communication inter-machines)
