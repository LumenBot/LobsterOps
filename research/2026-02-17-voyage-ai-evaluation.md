# Voyage AI — Évaluation pour LobsterOps
**Date:** 2026-02-17  
**Trigger:** @YannDecoopman (IZHC) setup de mémoire persistante multi-agents  
**Décision requise:** L2 (modification openclaw.json pour `memorySearch.provider = "voyage"`)

---

## Qu'est-ce que Voyage AI ?

API d'embeddings sémantiques recommandée par Anthropic. Convertit du texte en vecteurs pour la recherche sémantique — synonymes, contexte, pas juste du keyword matching.

**Cas d'usage :** indexer un vault de fichiers .md pour que les agents retrouvent des informations par sens, pas par mot-clé exact. Ex : chercher "coût d'utilisation Claude" trouve aussi les fichiers mentionnant "token budget", "prix API", "360×".

---

## Pricing

| Modèle | Prix/million tokens | Free tier |
|--------|-------------------|-----------|
| voyage-4-large | $0.12 | 200M tokens gratuits |
| voyage-4 | $0.06 | 200M tokens gratuits |
| **voyage-4-lite** | **$0.02** | **200M tokens gratuits** |
| voyage-context-3 | $0.18 | 200M tokens gratuits |
| rerank-2.5 | $0.05 | 200M tokens gratuits |

**Pour notre volume (~150KB, ~50 fichiers) :**
- 150KB de texte ≈ ~37 500 tokens
- Free tier = 200 000 000 tokens
- Ratio : **37 500 / 200 000 000 = 0.019%** du free tier
- Coût après free tier : ~$0.00075 par indexation complète du vault
- **Verdict pricing : quasi gratuit, le free tier suffit pour des années d'usage normal**

---

## Intégration OpenClaw

**Support natif confirmé** (docs.openclaw.ai/reference/api-usage-costs) :

```
memorySearch.provider = "voyage"
```

OpenClaw supporte nativement :
- `memorySearch.provider = "voyage"` ← Voyage AI
- `memorySearch.provider = "openai"` ← OpenAI
- `memorySearch.provider = "gemini"` ← Gemini
- `memorySearch.provider = "local"` ← local, zéro API

Intégration = modification `openclaw.json` → **L2 approval requis**.

---

## Contraintes Data Privacy

### Ce qui est envoyé à Voyage AI
- Le **texte brut** de chaque fichier est envoyé pour générer les embeddings
- Voyage AI crée les vecteurs et les retourne — le texte n'est **pas stocké** dans un index permanent (contrairement à Supermemory)
- Files API : fichiers retenus 30 jours (usage batch uniquement, pas pertinent ici)

### ⚠️ Avertissement @YannDecoopman
*"Ne stockez PAS vos clés et accès dans le vault car ça se retrouve indexé par Voyage AI."*

### Ce que ça implique pour notre vault
| Fichier | Sensibilité | Verdict |
|---------|------------|---------|
| `research/lobsterops/*.md` | 🟢 Faible | OK à envoyer |
| `memory/YYYY-MM-DD.md` | 🟡 Moyen | Contexte opérationnel |
| `MEMORY.md` | 🟡 Moyen | Profil Blaise, préférences |
| `SOUL.md`, `AGENTS.md` | 🟢 Faible | Architecture, règles |
| `vault/lessons/*.md` | 🟡 Moyen | Incidents, leçons |
| **Credentials** | 🔴 JAMAIS | Déjà dans env vars, jamais dans les fichiers |

**Notre situation actuelle :** Credentials déjà en env vars (✅ conforme). Le reste est du contexte opérationnel — pas d'informations hautement sensibles dans les fichiers actuels.

**Différence vs Supermemory :** Voyage AI crée des vecteurs depuis le texte, mais ne stocke pas les conversations/contexte dans une base de données tierce. C'est une embedding API, pas un service de mémoire persistante cloud.

---

## Alternatives Self-Hosted

| Option | Setup | Qualité | Coût |
|--------|-------|---------|------|
| `memorySearch.provider = "local"` (OpenClaw natif) | Zéro | Inférieure | $0 |
| Ollama + nomic-embed-text | Moyen | Bonne | $0 (CPU) |
| sentence-transformers (Python) | Complexe | Très bonne | $0 (CPU) |
| **Voyage AI** | Faible (config) | Excellente | ~$0 (free tier) |

**Notre VPS** : 1-2 vCPUs → modèles locaux = lent. Voyage AI = 200ms par batch → clairement supérieur pour notre setup.

---

## Setup de Yann Decoopman (Pattern IZHC)

1. **Obsidian** (.md liés logiquement) ← on fait déjà
2. **Bilan quotidien standardisé** par agent ← on fait déjà (memory/YYYY-MM-DD.md)
3. **Voyage AI** pour indexation sémantique du vault entier
4. **Propagation automatique** : quand un agent indexe, la connaissance est accessible à tous
5. **PDF → .md** avant indexation (pour les documents importants)

**Différenciateur clé** : *"Quand un agent cherche un truc, il indexe la mémoire pour tous les autres — la connaissance se propage entre agents automatiquement."*

---

## Comparaison vs Notre Setup Actuel

| Aspect | Actuel (ClawVault + memory-core) | Avec Voyage AI |
|--------|----------------------------------|----------------|
| Type de search | Tools manuels + FTS5 (keyword) | Sémantique (synonymes, contexte) |
| Déclenchement | Manuel (tool call) | Configurable (hooks ou manual) |
| Qualité retrieval | Bonne (107 nodes graph) | Excellente (+31.7pp benchmark) |
| Multi-agent | Via Canal Direct | Automatique (shared vault indexé) |
| Coût | $0 | ~$0 (free tier) |
| Privacy | Local total | Texte envoyé pour embedding |
| Setup | Déjà fait | config change L2 |

---

## Recommandation

**Score risque/bénéfice : FAVORABLE** — contrairement à Supermemory (cloud mémoire persistante), Voyage AI est une API d'embeddings stateless. Le texte est envoyé pour créer des vecteurs, pas stocké dans une base de données tierce interrogeable.

**⚠️ Condition préalable :** Audit rapide du vault pour s'assurer qu'aucune credential n'est dans les fichiers .md (déjà vérifié : credentials en env vars ✅).

**Proposition L2 :**
1. Ajouter `VOYAGE_API_KEY` en env var (signup gratuit sur voyageai.com)
2. Modifier `openclaw.json` : `memorySearch.provider = "voyage"`, model `voyage-4-lite`
3. Tester sur Ralph uniquement d'abord (pas The Constituent)
4. Évaluer qualité des résultats memory_search vs actuel

**Modèle recommandé :** `voyage-4-lite` ($0.02/million, free tier 200M) — suffisant pour notre volume, le meilleur rapport qualité/prix pour le use case.

---

## Action Required

**L2 — Blaise approval needed** avant tout changement.  
Si approuvé, setup estimé : 15 min (signup + config change + test).
