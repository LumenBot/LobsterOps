# Model Backup Fix + MiniMax M2.5 Investigation

**Date:** 2026-02-15 06:10 UTC  
**Context:** Model fallback cassé (`moonshotai/kimi-k2:free` unknown model error)  
**Task:** Fix urgence + investiguer MiniMax M2.5 comme alternative

---

## 🚨 Problème Identifié

### Config Actuelle (openclaw.json)
```json
"agents": {
  "defaults": {
    "model": {
      "primary": "anthropic/claude-sonnet-4-5",
      "fallbacks": ["moonshotai/kimi-k2:free"]
    },
    "models": {
      "openrouter/moonshotai/kimi-k2:free": { "alias": "kimi" },
      "moonshotai/kimi-k2:free": { "alias": "kimi" }
    }
  }
}
```

### Erreur
```
Unknown model: moonshotai/kimi-k2:free
```

### Cause
Le fallback utilise `moonshotai/kimi-k2:free` (sans prefix `openrouter/`), mais OpenClaw ne sait pas router ce format sans provider explicit. Deux alias définis mais fallback pointe vers la mauvaise variante.

---

## 🔍 MiniMax M2.5 — Investigation

### Release Timeline
- **11 fév 2026** : MiniMax M2.5 + M2.5 Lightning released
- **14-15 fév 2026** : Coverage VentureBeat, OpenHands, TeamDay
- **Disponibilité** : OpenRouter (`minimax/minimax-m2.5`)

### Performance (vs Claude Opus 4.6)
| Metric | Score | Notes |
|--------|-------|-------|
| SWE-Bench Verified | 80.2% | SOTA pour coding tasks |
| Multi-SWE-Bench | 51.3% | Multi-file editing |
| BrowseComp | 76.3% | Web automation |
| **Qualité** | ≈ Claude Sonnet 4.5 | Near-frontier performance |
| **Prix** | 1/10 à 1/20 Opus | $10K/year pour 4 agents continus |

### Caractéristiques Techniques
- **Open weights** (contrairement à Claude proprietary)
- **Reasoning-enabled** (planning + step-by-step thinking)
- **Token efficient** (optimisé pour actions/output)
- **Training** : Real-world digital working environments

### Sources
1. VentureBeat (14 fév) : "Near SOTA while costing 1/20th of Claude Opus 4.6"
2. OpenHands (11 fév) : "Open weights model up to Claude Sonnet quality"
3. TeamDay AI : "V3.2 matches frontier at 1/100th cost — value king 2026"
4. OpenRouter : Disponible `minimax/minimax-m2.5`

---

## 💡 Solutions Proposées

### Option 1 : Fix Minimal (Kimi uniquement)
**Action** : Corriger le prefix dans fallbacks
```json
"fallbacks": ["openrouter/moonshotai/kimi-k2:free"]
```

**Avantages** :
- ✅ Fix immédiat, zero risk
- ✅ Garde setup actuel

**Inconvénients** :
- ⚠️ Kimi-K2 = #1 OpenRouter mais pas testé pour fallback
- ⚠️ Performance inconnue vs Sonnet pour nos usages

---

### Option 2 : Switch vers MiniMax M2.5
**Action** : Remplacer Kimi par MiniMax M2.5
```json
"model": {
  "primary": "anthropic/claude-sonnet-4-5",
  "fallbacks": ["openrouter/minimax/minimax-m2.5"]
},
"models": {
  "anthropic/claude-sonnet-4-5": { "alias": "sonnet" },
  "openrouter/minimax/minimax-m2.5": { "alias": "minimax" }
}
```

**Avantages** :
- ✅ Performance proche Sonnet (≈80% SWE-Bench)
- ✅ 1/10 à 1/20 du coût Opus
- ✅ Reasoning-enabled (planning built-in)
- ✅ Fresh release (11 fév 2026), momentum fort
- ✅ SOTA pour coding tasks

**Inconvénients** :
- ⚠️ Nouveau modèle, moins battle-tested que Kimi
- ⚠️ Besoin validation comportement OpenClaw

---

### Option 3 : Dual Fallback (Kimi + MiniMax)
**Action** : Deux fallbacks en cascade
```json
"fallbacks": [
  "openrouter/minimax/minimax-m2.5",
  "openrouter/moonshotai/kimi-k2:free"
]
```

**Avantages** :
- ✅ Best of both worlds
- ✅ MiniMax try first (performance + cost)
- ✅ Kimi backup si MiniMax unavailable
- ✅ Résilience maximale

**Inconvénients** :
- ⚠️ Légèrement plus complexe
- ⚠️ Need to test fallback cascade behavior

---

## 🎯 Recommandation

**Option 3 (Dual Fallback)** pour production robustness :

```json
"agents": {
  "defaults": {
    "model": {
      "primary": "anthropic/claude-sonnet-4-5",
      "fallbacks": [
        "openrouter/minimax/minimax-m2.5",
        "openrouter/moonshotai/kimi-k2:free"
      ]
    },
    "models": {
      "anthropic/claude-sonnet-4-5": { "alias": "sonnet" },
      "openrouter/minimax/minimax-m2.5": { "alias": "minimax" },
      "openrouter/moonshotai/kimi-k2:free": { "alias": "kimi" }
    }
  }
}
```

**Rationale** :
1. MiniMax M2.5 = meilleur rapport qualité/prix du marché (VentureBeat)
2. Kimi-K2 = safety net éprouvé (#1 OpenRouter)
3. Fix le bug Kimi (prefix manquant)
4. Future-proof (MiniMax momentum fort feb 2026)

**Validation needed** :
- Test MiniMax fallback trigger (kill Anthropic temporairement)
- Vérifier behavior reasoning tokens MiniMax
- Benchmark latency vs Kimi

---

## 🔧 Patch Config (Ready to Apply)

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "anthropic/claude-sonnet-4-5",
        "fallbacks": [
          "openrouter/minimax/minimax-m2.5",
          "openrouter/moonshotai/kimi-k2:free"
        ]
      },
      "models": {
        "anthropic/claude-sonnet-4-5": { "alias": "sonnet" },
        "openrouter/minimax/minimax-m2.5": { "alias": "minimax" },
        "openrouter/moonshotai/kimi-k2:free": { "alias": "kimi" }
      }
    }
  }
}
```

**Prêt pour `gateway config.patch` dès que Blaise approuve.**

---

## 📊 MiniMax M2.5 — Deep Dive

### Positioning
- **Target** : "Real-world productivity" (OpenRouter)
- **Training** : "Diverse complex digital working environments"
- **Competitors** : Claude Sonnet 4.5, GPT-5, DeepSeek-V3.2

### Cost Analysis (VentureBeat)
- **Enterprise claim** : 4 agents × 1 year continuous = ~$10K
- **vs Opus** : 1/10 à 1/20 du coût
- **Implication** : Ralph + Constituent heartbeat 2min permanent = cost-effective long-term

### Adoption Signals
- Medium tutorial (11 fév) : "Easiest way to try cloud models" (Ollama cloud)
- TeamDay AI : "V3.2 value king 2026"
- OpenHands early access : "Basically up to Claude Sonnet quality"

### Red Flags (à surveiller)
- ⚠️ Trop récent (3 jours), besoin validation field
- ⚠️ Pas de benchmark LobsterOps-specific (veille, multi-agent)
- ⚠️ Reasoning tokens billing (unclear if extra cost)

---

## 📅 Next Steps

1. **Blaise approval** : Quelle option (1/2/3) ?
2. **Apply patch** : `gateway config.patch` + restart
3. **Test fallback** : Trigger MiniMax (kill Anthropic API 30s)
4. **Benchmark** : Compare response quality Ralph heartbeat MiniMax vs Sonnet
5. **Document** : Update MEMORY.md learnings post-validation

**ETA fix** : <5 min après approval.
