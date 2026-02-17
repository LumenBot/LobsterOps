# Memory Architecture — Deux Articles IZHC (17 février 2026)

**Source:** Discord IZHC  
**Reçu:** 2026-02-17 ~08:50 UTC  
**Tags:** memory, soul-md, agents-md, supermemory, hooks, vector-graph

---

## Article 1 : "Your OpenClaw Keeps Forgetting Everything. Here's How to Fix It."

**Source:** founder.codes (IZHC)  
**Thèse:** OpenClaw n'a pas de mémoire built-in — c'est une feature, pas un bug. Tu décides quoi retenir, où c'est stocké, qui y accède.

### Le système à 6 fichiers

| Fichier | Rôle | Key Insight |
|---------|------|-------------|
| `SOUL.md` | Personnalité, voix, limites | Section "NOT" > section "IS". Ajouter à chaque annoyance. Mettre les security boundaries ici |
| `IDENTITY.md` | Nom + rôle spécifique | "Chief of Staff" > "helpful assistant". Le nom crée de l'accountability |
| `USER.md` | Qui est l'humain, ses préférences | Empêche le "corporate assistant mode" |
| `TOOLS.md` | Outils + workarounds + ce qui NE marche PAS | Rule at top: "Check BEFORE saying I can't." Documenter les edge cases (X API, TikTok pipeline) |
| `MEMORY.md` | Long-term contextuel (projets, décisions) | Private uniquement. L'agent le maintient lui-même. Par mois 3 = plus de contexte qu'un humain après 1 an |
| `AGENTS.md` | Correction loop — chaque erreur = règle permanente | "No mental notes, write to file." Le fichier le plus important pour la progression |

### La Correction Loop (AGENTS.md)

Chaque fois que tu te surprends à penser "je te l'ai déjà dit" → c'est dans AGENTS.md.  
Pas de "je m'en souviendrai" — écrire dans un fichier immédiatement.

Exemples concrets documentés :
- Repos clonés partout → règle : cloner dans `/tmp/`, supprimer après PR
- Processus background qui meurent → règle : utiliser tmux
- "Je ne peux pas voir l'image" → "figure it out" → l'agent a cherché et résolu
- Flags CLI incorrects → documenter la syntaxe exacte

### "Figure it out" > instructions détaillées

*"If you tell the AI the answer, it learns the answer. If you tell it to figure it out, it learns how to solve problems."*

### Skills

Skills = playbooks spécialisés chargés à la demande. Exemple Rory (content writer) :
- `copywriting.md` — chargé pour landing pages/headlines
- `email-sequence.md` — chargé pour drip campaigns
- `humanizer.md` — runs sur tout output avant livraison (em dash, "it's important to note", rule of three)

**Statut LobsterOps :** Système quasi-identique déjà en place. Points de validation :
- ✅ SOUL.md + USER.md + TOOLS.md + MEMORY.md + AGENTS.md + daily logs
- ✅ Skills (clawvault, github, tmux, coding-agent, etc.)
- ✅ Private MEMORY.md (pas dans group chats)
- 🔁 Correction loop : à renforcer (ajouter systématiquement dans AGENTS.md, pas juste SOUL.md)

---

## Article 2 : "Why everyone is complaining about OpenClaw's memory (it sucks) - and why supermemory fixes it"

**Source:** supermemoryai (IZHC), Pedro Dias + Dhravya Shah  
**Repo :** github.com/supermemoryai/openclaw-supermemory  
**Stats :** Plugin v1 = 500K views à son lancement

### Anatomie du problème mémoire OpenClaw

**Deux couches de storage (natif) :**
- `memory/YYYY-MM-DD.md` — logs quotidiens append-only (scratchpad courant)
- `MEMORY.md` — faits long-terme, préférences, décisions (DM uniquement)

**Deux outils (read-side) :**
- `memory_search` — semantic search sur tous les fichiers mémoire
- `memory_get` — lire lignes spécifiques après search

**Les 4 problèmes fondamentaux :**

1. **Tools-based = lent + cher** — Chaque opération mémoire = tool call = tokens + latence. Plus de tokens que d'ignorer la mémoire parfois.
2. **Pas de knowledge updates** — L'agent ne sait pas ce qui est déjà en mémoire. Résultat : infos redondantes, pas de mise à jour des faits existants, pas de construction cumulative.
3. **Pas de temporal reasoning** — Ne sait pas ce qui est récent vs obsolète.
4. **Ne oublie jamais rien** — Le forgetfulness est une feature critique pour garder le contexte frais. Sans pruning, la mémoire se dilue.

**Signal confirmé :** Même avec QMD installé, @levelsio (15 fév.) continue d'avoir le problème. La communauté recommande massivement Supermemory à la place.

### Comment Supermemory fixe ça

**Architecture différente :**
- **Hooks** (pas tools) → saves implicites en background, zéro friction
- **Vector-graph layer** → chaque fact est contextualisé par rapport aux autres
- **Knowledge updates automatiques** → détecte les contradictions, met à jour les faits existants
- **Temporal reasoning** → sait distinguer récent vs obsolète
- **Forgetfulness built-in** → pruning automatique pour garder le contexte frais

**Auto-routing + containerTags** (v2, sortie aujourd'hui) → meilleure configurabilité

**Benchmarks annoncés** (pas encore publiés dans l'article partagé)

### Évaluation LobsterOps

| Critère | Situation actuelle | Avec Supermemory |
|---------|-------------------|-----------------|
| Storage | ClawVault (fichiers md + graph 107 nodes) | Vector-graph cloud |
| Save mode | Manual tool calls | Hooks implicites |
| Knowledge updates | Manuel (edits MEMORY.md) | Auto-détecté |
| Temporal reasoning | Absent | Built-in |
| Forgetfulness | Absent (accumulation) | Auto-pruning |
| Privacy | 100% local | Cloud (supermemory.ai) |
| Setup | Déjà fait (ClawVault) | L2 installation requise |

**Verdict :** Solution intéressante mais **trade-off critique** : cloud vs local. Notre posture sécurité (data ownership, local-first) est en tension directe avec un service cloud memory. À proposer à Blaise (L2) si on veut évaluer — pas d'installation autonome.

---

## Actions Recommandées

| Action | Priorité | Niveau |
|--------|----------|--------|
| Renforcer correction loop (AGENTS.md) — documenter les erreurs systématiquement | 🟡 MOYENNE | L1 autonome |
| Évaluer Supermemory plugin (local vs cloud trade-off) | 🟡 MOYENNE | L2 (proposer à Blaise) |
| Vérifier notre TOOLS.md — bien documenté les workarounds ? | 🟢 FAIBLE | L1 autonome |
