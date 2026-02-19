# Ralph — MEMORY.md
> Dernière MàJ : 2026-02-19

---

## Opérateur — Blaise

- Profil : technique avancé (CLI, Docker, TypeScript/Node.js), basé Nancy
- Projet : LobsterOps — expertise OpenClaw + systèmes multi-agents
- Transition pro : nouveau poste 2 mars 2026
- Infra : VPS DigitalOcean 2vCPU/4GB
- Comm : français par défaut, tutoiement, direct, pas de flatterie
- Travail : esprit critique, vision transversale, orientation opérationnelle

---

## Docs de référence LobsterOps (8 docs, 6500+ lignes)

| Doc | Contenu |
|-----|---------|
| Encyclopedia | Architecture OpenClaw, écosystème |
| Playbook | Guide opérationnel Phase 0-6 |
| Deep Dives | Annexes techniques avancées |
| Ecosystem Watch | Releases, articles, signaux (chronologique) |
| Crypto doc | Agents × crypto ecosystem |
| Knowledge Index | Navigation transversale |
| Service Delivery | Workflow Phase 0→4 |
| Guide WSL2 | Setup Windows |

---

## OpenClaw — Faits clés (fév 2026)

- 179K GitHub stars, créé par Steinberger (@steipete)
- Steinberger → OpenAI, Foundation MIT — mainstream adoption amorcée
- 135K+ instances exposées, ~900 skills malveillants sur ClawHub
- Recommandation steipete : Anthropic Pro/Max + Opus 4.6
- Alerte : Opus 4.6 = token hungry. Tiering : Opus (complexe), Sonnet (courant), Haiku/local (simple)

---

## Heuristiques opérationnelles

- "If you cannot verbalize it, you cannot automate it"
- "Constraints > freedom" — instructions spécifiques > ouvertes
- "Define done explicitly" — critères d'acceptation par tâche
- ">2x = make it a Skill"
- "Zero-based budgeting for prompts" — vider et reconstruire tous les 3 mois

---

## Leçons apprises

### 2026-02-14 — Multi-Agent Deployment
- Migration Python → OpenClaw native faisable en <12h
- Telegram routing multi-bots via `channels.telegram.accounts` + `bindings`
- Workspaces isolés = isolation sessions parfaite
- Multi-agent config = L2 (propose → validate → apply), jamais L1

### 2026-02-15 — Session Persistence & Fallback
- Sessions actives retiennent leur model après SIGUSR1 (zero downtime)
- Fallback cascade QUE pour nouvelles sessions OU API failure OU full restart
- Toujours prefix `openrouter/` pour models non-Anthropic dans fallbacks

### 2026-02-16 — Gateway Crash (openclaw.json)
- Une modification mal validée = gateway crash = agents injoignables
- Règle : backup obligatoire, `openclaw doctor`, L2 approval systématique

### 2026-02-18 — Config Cascade Crash (90min downtime)
- 4 crashes consécutifs : thinking blocks, modèle mort kimi-k2:free, clé "providers" invalide, bindings mal placés
- Règle L3 : aucune modif openclaw.json sans diff temporaire → doctor → test Constituent → backup → apply
- SIGUSR1 ne recharge PAS auth-profiles (full restart requis)
- `nohup kill -USR1 $(pgrep -f openclaw-gateway) &` (jamais kill direct depuis exec)

### 2026-02-19 — Perte accès LLM
- Anthropic rate-limited/révoqué, Groq clé invalide, Ollama trop lourd pour VPS 4GB
- Groq free tier = 12K TPM limit — insuffisant si contexte > 12K tokens
- Groq Dev tier = solution viable
- Toujours tester clé API avec curl avant de configurer dans openclaw.json
- Utiliser Python (json.load/dump) pour éditer openclaw.json, JAMAIS sed

---

## Agents déployés

| Agent | Workspace | Mission | Status |
|-------|-----------|---------|--------|
| Ralph (main) | workspace/ | Veille, recherche, orchestration | Actif |
| The Constituent | workspace-constituent/ | Staging/test environment | Standby |
| Client Instagram | workspace-client/ | @napolille (Gautier) | Actif |
| Scout | workspace-paris/ | Paris sportifs (advisory) | Actif |

### Architecture validée
- Pattern Orchestrator + Specialists proven
- Canal Direct via workspace-shared/ (file drops)
- 1 agent + skills > N agents confus
- Déploiement : Constituent (staging) → Ralph → Agents clients (production)
