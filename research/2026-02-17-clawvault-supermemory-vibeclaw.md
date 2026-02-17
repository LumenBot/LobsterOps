# IZHC Articles — ClawVault v2.6.0 + Supermemory Benchmarks + Vibeclawdbotting (17 fév 2026)

**Source:** Discord IZHC  
**Tags:** clawvault, supermemory, memory-bench, primitives, autonomy, marketing-automation

---

## 1. Supermemory Benchmarks (suite article précédent)

**Repo MemoryBench :** github.com/supermemoryai/memorybench (open source)

| Système | Score | Mode |
|---------|-------|------|
| Filesystem (Claude Code) | 54.2% | Manuel |
| OpenClaw native RAG | 58.3% | Tool call (si appelé) |
| **Supermemory** | **85.9%** | Automatique (hooks) |

**+31.7 points de pourcentage** au-dessus d'OpenClaw natif.  
Aussi : moins de tokens, plus rapide, moins cher.  
Prix : $20/mois.

**Statut LobsterOps :** Avantage réel confirmé par benchmarks. Mais trade-off cloud reste entier. L2 requis pour installation.

---

## 2. ClawVault v2.6.0 — "Solving Long-Term Autonomy"

**Stats 72h :** 12 releases, 459 tests.

### Changement fondamental : YAML schema templates

Chaque primitive (task, project, decision, lesson, people) = template YAML malleable.

```yaml
primitive: task
fields:
  status: { type: string, required: true, default: open, enum: [open, in-progress, blocked, done] }
  priority: { type: string, enum: [critical, high, medium, low] }
  owner: { type: string }
  due: { type: date }
  tags: { type: string[] }
  depends_on: { type: string[] }
```

**Clé : malleable.** Ajouter un champ = éditer un .md. Pas une PR. L'agent lit TON schema, pas le leur.

**Nouveau skill :**
```bash
clawhub install agent-autonomy-primitives
```
Couvre les 5 primitives + heartbeat loops + personnalisation templates.

### Trigger-based autonomy (vs cron)

Pattern démontré :
1. Email entrant détecté → `clawvault task add` avec contexte
2. Agent cherche en mémoire : style de communication, décisions passées, leçons
3. Exécute, répond, ferme la tâche
4. Stocke la leçon : "Justin's shipping questions always need tracking numbers"

**Cycle :** Event → Task → Memory → Execution → Lesson → next cycle smarter

### Obsidian comme control plane

Toutes les primitives = markdown + YAML frontmatter = visibles dans Obsidian.  
5 vues générées automatiquement : All tasks (Kanban), Blocked, By project, By owner, Backlog.  
Le filesystem = l'intégration. Pas de sync layer, pas de dashboard.

### Multi-agent shared vault

Clawdious (ops) + Eli (client-facing) partagent le même vault :
- Pas de message passing, pas d'API
- Juste deux agents qui lisent/écrivent dans le même filesystem
- **Compatible avec notre protocole Canal Direct**

### Pertinence LobsterOps : ⭐⭐⭐⭐⭐

- Notre ClawVault (v2.5.11) → v2.6.0 = upgrade majeur (YAML schemas)
- Trigger-based pattern = notre prochain step d'autonomie
- Obsidian integration = dashboard gratuit sur notre vault
- Multi-agent shared vault = valide notre architecture Ralph/Constituent
- **Action requise (L2) :** Proposer upgrade ClawVault v2.5.11 → v2.6.0 à Blaise

---

## 3. Vibeclawdbotting — Marketing Automation avec OpenClaw

**Source :** IZHC, auteur non identifié

### 10 use cases documentés

| # | Use Case | Résultats | Notes Éthiques |
|---|----------|-----------|----------------|
| 1 | Buying intent sniping (X/Reddit/Quora) | ~50 visites/1K views, backlinks 1-2 ans | ⚠️ Gris (automated replies) |
| 2 | AI slop machine (20+ plateformes) | 200+ visites/jour en 1 semaine, $1/topic | 🔴 Gray hat (contenu bulk auto) |
| 3 | Directory submissions (100+) | SEO + LLM citations | 🟢 OK |
| 4 | TikTok content factory | 500K views en 5 jours, $0.50/post (Oliver Henry method) | 🟡 Semi-manuel |
| 5 | Autonomous X reply guy | 400 followers en 4 jours, 7 démos. **Max 200 replies/jour** au-delà = ban | ⚠️ Gris |
| 6 | Job posting sniper | Pitch "avant de recruter, essaie notre agent" | 🟡 B2B outreach |
| 8 | SEO keyword gap | Contenu généré sur gaps concurrents | 🟡 OK si qualité |
| 9 | Telegram/Discord infiltration | "15+ Discords marketing et SaaS" | 🔴 Non éthique |
| 10 | Self-improving skill files | Skill files qui s'améliorent session après session | ✅ Pattern excellent |

### Pattern "Self-improving skill files" (le seul vraiment pertinent pour nous)

Ce que contient un skill file marketing efficace :
- Templates d'outreach qui ont eu des replies (et ceux bannis)
- Hook formulas qui ont performé (et ceux qui ont floppé + pourquoi)
- Structure d'articles SEO qui rankent
- Tone par plateforme (Reddit ≠ LinkedIn ≠ X)
- DM templates → demos vs ban

Principe : "When something fails — add a rule. When something succeeds — add a formula."  
→ **C'est exactement notre pattern AGENTS.md / correction loop.** Valide la philosophie.

### Pertinence LobsterOps

- ❌ La plupart des use cases = gray/black hat, pas pour nous (infiltration, spam, slop)
- ✅ Pattern self-improving skill files = applicable directement (on le fait déjà)
- ✅ Buying intent monitoring passif = potentiellement utile pour TAR (monitorer les discussions sur TheAgentsRepublic sans spam)
- 🔭 TikTok factory via Oliver Henry = à suivre si on crée du contenu LobsterOps

---

## Actions Recommandées

| Action | Priorité | Niveau |
|--------|----------|--------|
| **Proposer upgrade ClawVault v2.5.11 → v2.6.0** (YAML schemas, trigger-based, Obsidian) | 🔴 HAUTE | L2 — proposer à Blaise |
| Installer `agent-autonomy-primitives` skill (si upgrade approuvé) | 🟡 MOYENNE | L2 (package install) |
| Supermemory plugin évaluation (benchmarks +31.7pp, $20/mois, cloud) | 🟡 MOYENNE | L2 — proposer à Blaise |
| Éviter tous les use cases vibeclawdbotting gray/black hat | N/A | Hors scope éthique |
