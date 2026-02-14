Ralph — MEMORY.md

Ce fichier contient les connaissances persistantes de Ralph.
Il est lu à chaque session. Chaque correction et leçon apprise y est documentée.
Dernière MàJ : 13 février 2026


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

PicoClaw comparison (fork 4.5K stars en 2j, hardware $10, migration OpenClaw)
Mimiclaw edge deployment pattern (ESP32-S3, IoT/edge AI)
Collective Intelligence patterns (Spark, 166 agents)
Clawathon résultats
ClawRouter audit sécurité wallet USDC
Benchmark Opus 4.6 vs 4.5 token consumption
Guide "basic → production"
Polymarket trading bot pattern (self-healing, $15 overnight)


OpenClaw — Faits clés
Version actuelle recommandée
v2026.2.12 (13 février 2026) — 40+ security fixes (sandboxing, SSRF, auth bypass), GLM-5 + MiniMax M2.5 support
Statistiques

179K GitHub stars, 29.7K forks (10 fév. 2026)
Créé par Peter Steinberger (@steipete)

Sécurité — Situation critique (fév. 2026)

135K+ instances exposées sur Internet (hausse exponentielle)
~900 skills malveillants sur ClawHub (~20% du registre)
283 skills exposent des credentials en clair
ZeroLeaks score : 2/100 (très vulnérable aux injections)
Architecture sécurité recommandée : 7 couches (SHIELD.md → ClawSec → Docker → Tailscale)

Outils sécurité validés (fév. 2026)

**Cisco Skill Scanner** — Analyse statique des skills (code malveillant, fuites credentials)
**VirusTotal ClawHub Integration** — Scan automatique skills sur ClawHub
**Safety Scanner** — Feature native v2026.2.6 (analyse runtime)
**OpenClaw Scanner** — Outil discovery instances exposées (Shodan/ZoomEye)

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

### 2026-02-12 — Clé API exposée dans le chat Telegram
**Erreur :** Ralph a affiché la clé Brave Search API en clair dans une réponse Telegram (message 15:27 UTC : "BSAUUWi_for8noVz_Kn3P_WtfXt3uEu")  
**Cause :** Pas de règle explicite dans SOUL.md sur le masquage des credentials  
**Fix :** Ajout dans SOUL.md section "Ce que tu NE fais PAS" : "Tu n'affiches JAMAIS de clés API, tokens ou credentials en clair dans tes réponses"  
**Règle :** Toujours masquer les secrets — ne jamais les afficher même partiellement. Format acceptable : `BSA***Eu` ou `[REDACTED]`

### 2026-02-12 — Grok comme source non fiable
**Erreur :** Stats ShieldClaw provenant de Grok (64% prod sans sécurité, 16% compromis, $34K pertes) non vérifiables via sources web publiques  
**Cause :** Grok peut halluciner des noms d'outils et des statistiques d'adoption sans sources citables  
**Fix :** ShieldClaw déplacé en backlog "à valider" dans Ecosystem Watch, en attente de confirmation par sources indexées  
**Règle :** Toujours cross-checker les infos Grok avec des sources web indexées (Brave Search, articles, GitHub) avant d'intégrer dans les docs. Grok = hypothèse, pas fait.

---

<!-- Template pour futures leçons :
### [Date] — [Titre court]
**Erreur :** [Ce qui s'est passé]
**Cause :** [Pourquoi]
**Fix :** [Ce qu'on a fait]
**Règle :** [Ce qu'on fait maintenant pour éviter que ça se reproduise]
