# Scout — Agent Paris Sportifs : Spec Technique Complète
> Créé le 2026-02-18 | Statut : L2 PENDING (approbation Blaise requise avant déploiement)
> Version 1.0

---

## 1. Vue d'ensemble

**Scout** est un agent OpenClaw spécialisé dans l'analyse des paris sportifs sur Winamax.
Il opère en mode **advisory only** : il recommande, les humains décident et placent.

| Paramètre | Valeur |
|-----------|--------|
| Agent ID | `scout` |
| Workspace | `/root/.openclaw/workspace-paris/` |
| Groupe Telegram | `-1003846367149` |
| Membres | Blaise, Gautier, Jules + bot |
| Sport | Football uniquement (v1.0) |
| Mode | Advisory only — L3 hard block sur toute exécution |
| Fréquence | 1x/jour à 09:00 UTC (avant matchs du jour) |
| Bot Telegram | **À créer via @BotFather** (token requis avant déploiement) |

---

## 2. Architecture Technique

```
┌─────────────────────────────────────────────────────────┐
│                    SCOUT AGENT                          │
├──────────────┬────────────────────┬────────────────────┤
│  DATA LAYER  │  ANALYSIS ENGINE   │  OUTPUT LAYER      │
│              │                    │                    │
│ API-Football │  Scoring Model     │  Telegram Group    │
│ (forme, H2H) │  Confidence 0-100  │  Daily 09:00 UTC   │
│              │                    │                    │
│ Playwright   │  Filtre cotes      │  6 sélections      │
│ (cotes       │  1.6-2.2           │  + 1 combiné       │
│  Winamax)    │                    │  recommandé        │
│              │  Bankroll calc     │                    │
│              │  1% / 2%           │                    │
└──────────────┴────────────────────┴────────────────────┘
```

### 2.1 Sources de données

#### API-Football (stats)
- **URL** : `https://v3.football.api-sports.io`
- **Free tier** : 100 requêtes/jour ✅ suffisant
- **Estimation usage** : ~40-50 req/jour pour 10 matchs
  - Fixtures du jour : 1 req
  - Form équipe (5 derniers) : 2 req × 10 matchs = 20 req
  - H2H : 1 req × 10 matchs = 10 req
  - Standings (contexte) : 5 req (une par ligue couverte)
- **Clé API** : À obtenir sur api-football.com (gratuit, inscription email)
- **Ligues prioritaires** : Ligue 1, Premier League, Liga, Serie A, Bundesliga, CL/EL

#### Playwright + Stealth (cotes Winamax)
- **Approche** : Browser automation Option B (page scraping)
- **URL cibles** : `https://www.winamax.fr/paris-sportifs/sports/1/` (football)
- **Anti-bot** : User-agent rotation + délais humains + viewport réaliste
- **Données extraites** : Cotes 1X2 + double chance + over/under goals
- **Fréquence scraping** : 1x/jour à 08:30 UTC (30min avant publication)
- **Fallback** : Si Winamax bloque → signalement dans le message groupe

---

## 3. Modèle de Scoring de Confiance

### 3.1 Composantes (score 0-100)

```
Score_Confiance = (Form × 0.40) + (H2H × 0.30) + (Valeur × 0.30)
```

#### Form Score (40% du total)

Pour chaque équipe, 5 derniers matchs :
- Victoire = 3 pts
- Nul = 1 pt
- Défaite = 0 pt
- Max = 15 pts par équipe

```python
def form_score(results_home, results_away, bet_type):
    # bet_type: "1X" | "X2" | "12"
    pts_home = sum(3 if r=="W" else 1 if r=="D" else 0 for r in results_home)
    pts_away = sum(3 if r=="W" else 1 if r=="D" else 0 for r in results_away)
    
    if bet_type == "1X":  # Domicile fort OU nul
        raw = (pts_home * 0.7) + (pts_away * 0.3)  # favorise forme domicile
    elif bet_type == "X2":  # Extérieur fort OU nul
        raw = (pts_home * 0.3) + (pts_away * 0.7)
    elif bet_type == "12":  # Un vainqueur (pas nul)
        raw = max(pts_home, pts_away)  # le plus fort des deux
    
    return (raw / 15) * 100  # normalisé 0-100
```

#### H2H Score (30% du total)

5 derniers matchs entre les deux équipes :
```python
def h2h_score(h2h_results, bet_type):
    # h2h_results: liste de [home_goals, away_goals] des 5 derniers
    wins_home = sum(1 for h, a in h2h_results if h > a)
    wins_away = sum(1 for h, a in h2h_results if a > h)
    draws = 5 - wins_home - wins_away
    
    if bet_type == "1X":
        favorable = wins_home + draws
    elif bet_type == "X2":
        favorable = wins_away + draws
    elif bet_type == "12":
        favorable = wins_home + wins_away
    
    return (favorable / 5) * 100  # 0 à 100
```

#### Valeur Score (30% du total)

Compare notre estimation de probabilité à la probabilité implicite de la cote :
```python
def value_score(odds, estimated_prob):
    # odds : cote Winamax (ex: 1.75)
    # estimated_prob : notre estimation basée forme + H2H (0.0 à 1.0)
    
    implied_prob = 1 / odds  # probabilité bookmaker
    value = (estimated_prob - implied_prob) / implied_prob  # surplus de valeur
    
    # Normaliser : value > 0.20 = 100pts, value < -0.10 = 0pts
    normalized = min(100, max(0, (value + 0.10) / 0.30 * 100))
    return normalized

def estimate_probability(form_score, h2h_score):
    # Combine form et H2H pour estimer notre probabilité
    return (form_score * 0.6 + h2h_score * 0.4) / 100
```

### 3.2 Filtres Goals

```python
def goals_filter(team_stats_home, team_stats_away, h2h_results):
    # Over 1.5 goals : au moins 80% des derniers matchs ont +2 buts
    avg_goals = [h+a for h, a in h2h_results]
    over_15_ok = sum(1 for g in avg_goals if g > 1.5) / len(avg_goals) >= 0.8
    
    # Under 4.5 goals : au moins 80% des matchs H2H ont <5 buts
    under_45_ok = sum(1 for g in avg_goals if g < 4.5) / len(avg_goals) >= 0.8
    
    return over_15_ok, under_45_ok
```

### 3.3 Seuils de décision

| Score confiance | Action | Mise recommandée |
|----------------|--------|-----------------|
| < 60 | Exclu de la sélection | — |
| 60-69 | Sélection possible | 1% bankroll |
| 70-79 | Bonne sélection | 1% bankroll |
| ≥ 80 | Top pick ⭐ | 2% bankroll |

### 3.4 Critères combiné (2 matchs max)

- Les 2 picks doivent avoir score ≥ 65
- Cote du combiné = cote1 × cote2, doit être entre **1.60 et 2.20**
- Si cote combiné > 2.20 → ne pas recommander le combiné
- 1 seul combiné par jour (le meilleur)

---

## 4. Format des Messages Telegram

### 4.1 Message quotidien (09:00 UTC)

```
🔍 SCOUT — Analyse du 18 fév. 2026
────────────────────────────────────

📊 TOP 6 SÉLECTIONS FOOTBALL

1. ⭐ PSG vs Lyon — Ligue 1
   Pari : Double chance 1X + Over 1.5 buts
   Cote Winamax : 1.82 | Confiance : 83/100
   Mise : 2% bankroll
   ├ Forme PSG : W W W D W (14/15) 🔥
   ├ Forme Lyon : L D W L W (7/15)
   └ H2H (5 der.) : PSG 3-0-2, 4.2 buts/match

2. Arsenal vs Man City — Premier League
   Pari : Double chance 12 + Over 1.5 buts
   Cote Winamax : 1.65 | Confiance : 76/100
   Mise : 1% bankroll
   ├ Forme Arsenal : W W D W W (13/15)
   ├ Forme Man City : W D W W L (10/15)
   └ H2H : 2-1-2, 2.8 buts/match

[... jusqu'à 6 sélections ...]

────────────────────────────────────
🎯 COMBINÉ DU JOUR (cote totale : 1.87)
   → Pick 1 + Pick 3
   Mise : 1% bankroll

────────────────────────────────────
💰 EXEMPLE BANKROLL 100€
   Mise standard (1%) : 1€/pari
   Mise max (2%) : 2€/pari

⚠️ Advisory only. Vérifiez les cotes avant de parier.
Nb matchs analysés : 14 | API-Football quota : 42/100
```

### 4.2 Message d'alerte (si scraping Winamax échoue)

```
⚠️ SCOUT — Données Winamax indisponibles ce matin
Le scraping a rencontré un blocage. Retry programmé dans 2h.
Si le problème persiste, vérifiez les cotes manuellement.
```

---

## 5. Architecture Agent OpenClaw

### 5.1 Fichiers de l'agent

```
/root/.openclaw/workspace-paris/
├── AGENTS.md          # Règles opérationnelles (voir §6)
├── SOUL.md            # Personnalité Scout (voir §7)
├── CONTEXT.md         # Contexte initial (voir §8)
├── HEARTBEAT.md       # Cron quotidien
├── MEMORY.md          # Mémoire long terme
├── memory/            # Logs quotidiens
│   └── heartbeat-state.json
├── data/              # Données scraping
│   ├── winamax-odds-YYYY-MM-DD.json
│   └── api-football-cache/
└── scripts/           # Scripts d'analyse
    ├── scrape_winamax.py
    ├── fetch_stats.py
    └── scoring.py
```

### 5.2 Modification openclaw.json requise (L2 — diff proposé)

```json
// Ajouter dans agents.list :
{
  "id": "scout",
  "name": "Scout",
  "workspace": "/root/.openclaw/workspace-paris",
  "heartbeat": {
    "every": "30m"
  },
  "tools": {
    "allow": [
      "read", "write", "edit", "exec", "process",
      "web_search", "web_fetch", "browser", "message",
      "cron", "image"
    ],
    "deny": [
      "gateway", "sessions_send", "agents_list", "nodes"
    ]
  }
}

// Ajouter dans bindings :
{
  "agentId": "scout",
  "accountId": "scout",    // nouveau bot token
  "match": {
    "channel": "telegram",
    "peer": {
      "kind": "group",
      "id": "-1003846367149"
    }
  }
}

// Ajouter dans channels.telegram.accounts :
"scout": {
  "dmPolicy": "pairing",
  "botToken": "[TOKEN_BOTFATHER_À_FOURNIR]",
  "groupPolicy": "allowlist",
  "allowedGroupIds": ["-1003846367149"],
  "streamMode": "partial"
}
```

> ⚠️ **Requis avant déploiement** : Blaise crée le bot via @BotFather et fournit le token.

---

## 6. Prérequis déploiement

| Prérequis | Statut | Action |
|-----------|--------|--------|
| Bot Telegram "Scout" | ❌ À créer | Blaise → @BotFather |
| Token bot | ❌ À obtenir | Blaise → fournir à Ralph |
| Clé API-Football | ❌ À créer | Blaise → api-football.com (gratuit) |
| Groupe Telegram créé | ✅ | ID : -1003846367149 |
| L2 approval Blaise | ⏳ Pending | Ce document = demande d'approbation |

---

## 7. Limites connues v1.0

1. **Winamax anti-bot** : Playwright stealth peut être bloqué. Prévoir fallback + alerte groupe.
2. **API-Football free** : 100 req/jour. Pas de rétrocompatibilité si quota dépassé.
3. **Modèle de scoring simplifié** : Pas de données de blessures, suspensions, météo.
4. **Cotes dynamiques** : Les cotes changent jusqu'au coup de sifflet. Toujours vérifier.
5. **Pas de tracking P&L** : V1 ne suit pas les résultats. À ajouter en V2.

---

## 8. Roadmap V2 (après validation empirique ~4 semaines)

- Tracking P&L automatique (résultats des paris recommandés)
- Modèle ML simple (régression logistique sur historique résultats vs prédictions)
- Ajout tennis / basket sur demande
- Notification live avant coup d'envoi (1h avant match)
- Dashboard web simple (optionnel)
