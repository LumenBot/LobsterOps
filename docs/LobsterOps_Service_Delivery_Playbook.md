# LobsterOps — Service Delivery Playbook
## Workflow opérationnel : du prospect au déploiement agent client

**Version :** 1.1  
**Date :** 2026-02-18 (MàJ post-déploiements Gautier + Scout — leçons terrain)  
**Statut :** Référence opérationnelle (Mission 4)  
**Classification :** Interne LobsterOps — ne pas partager avec les clients tel quel

---

## 1. Vue d'Ensemble

Ce document formalise le processus complet de livraison d'un agent IA métier pour un client externe, de la prise de contact jusqu'à la maintenance en production.

**Acteurs :**
- **Blaise** — Signataire, commercial face-à-face, déployeur infra, superviseur
- **Ralph** — Expert technique, configurateur d'agent, formateur, moniteur
- **Client** — Donneur d'ordre, exécutant de commandes (guidé), utilisateur final
- **Agent Client** — L'agent IA déployé pour le client

**Principe fondamental :** Ralph est le moteur opérationnel. Blaise est le point de contact humain et le garant de la confidentialité. Le client interagit avec les deux.

---

## 2. Phase 0 — Pré-vente (Blaise uniquement)

### 2.1 Premier contact
- **Canal :** Email, LinkedIn, IZHC Discord, ou réseau direct
- **Objectif :** Qualifier le besoin (workflow à automatiser, budget, timeline)
- **Livrable :** Discovery call planifiée (30 min, Calendly)

### 2.2 Discovery call
- **Participants :** Blaise + Client
- **Objectif :** Comprendre le workflow, les pain points, les sources de données, les outils existants
- **Livrable :** Notes de discovery (markdown, stockées dans le repo client)
- **Décision :** Go/No-Go

### 2.3 Proposal
- **Rédacteur :** Ralph (sur instruction de Blaise, dans le workspace LobsterOps)
- **Contenu :** Use case, architecture agent proposée, timeline, pricing
- **Validation :** Blaise review (L2) avant envoi au client
- **Livrable :** Proposal PDF (5-10 pages)

### 2.4 Signature
- **Signataire :** Blaise (auto-entrepreneur)
- **Paiement :** 50% upfront via Stripe/Outseta, 50% à la livraison
- **Contrat :** SOW simple (2-3 pages)

---

## 3. Phase 1 — Setup Infrastructure (Blaise, ~2-3h)

### 3.1 Création de l'environnement client

Blaise exécute ces étapes manuellement :

```bash
# 1. Créer le VPS client (DigitalOcean, Hetzner, ou mutualisé)
# Specs minimum : 2 vCPUs, 2GB RAM, 20GB SSD, Ubuntu 24
# Région : au choix du client (Europe par défaut)

# 2. Installer OpenClaw sur le VPS client
npm install -g openclaw@latest
openclaw onboard --install-daemon

# 3. Configurer le gateway
# - Provider : Claude Max (token Blaise, facturé au client dans la maintenance)
# - OU : compte Claude du client (si le client a son propre abonnement)
# - Telegram bot dédié au client (via @BotFather)
# - Brave Search API (clé dédiée ou partagée)

# 4. Créer le repo GitHub privé
# Nom : lobsterops-client-[nom-client]
# Accès : Blaise (admin), Ralph (push via LobsterOps workspace)
# PAS d'accès direct du client au repo (sauf demande explicite)
```

### 3.2 Configuration Bot Telegram (⚠️ Étape critique — souvent oubliée)

**Avant d'ajouter le bot dans un groupe :**

```bash
# 1. Désactiver le privacy mode (sinon le bot ne voit pas les messages)
# @BotFather → /mybots → [bot] → Bot Settings → Group Privacy → Turn off

# 2. Obtenir l'ID du groupe Telegram
# Ajouter @userinfobot dans le groupe → il affiche le group ID (-100XXXXXXXXXX)

# 3. Modifier openclaw.json dans l'ordre EXACT suivant (L2 requis) :
# a. agents.list → ajouter l'agentId
# b. channels.telegram.groups → ajouter le group ID
# c. bindings → ajouter le binding peer.id → agentId
# d. agents.[agentId].workspaceDir → pointer vers le workspace

# Template channels.telegram.groups :
{
  "channels": {
    "telegram": {
      "groups": {
        "-1003XXXXXXXXX": {
          "requireMention": false   // true si agent ne doit répondre qu'aux mentions
        }
      }
    }
  }
}
```

**⚠️ RULE : channels.groups + binding + agents.list + privacy mode = 4 conditions simultanées.**  
L'omission d'une seule = agent sourd dans le groupe. (Leçon terrain 2026-02-18)

---

### 3.3 Créer le groupe Telegram client

```
Groupe Telegram : "LobsterOps × [Nom Client]"
Membres :
  - Blaise (admin, superviseur)
  - Ralph (agent LobsterOps, support technique)
  - Client (utilisateur)
  - [Agent Client] (ajouté en Phase 3 quand déployé)
```

### 3.4 Isolation et sécurité

| Élément | Règle |
|---------|-------|
| VPS client | Physiquement séparé du VPS Ralph |
| Repo GitHub | Privé, dédié, aucun lien avec lobsterops-ralph-workspace |
| Groupe Telegram | Dédié par client, aucun cross-posting |
| Credentials client | Jamais stockés dans le workspace Ralph |
| Données client | Jamais dans le ClawVault de Ralph |
| Workspace Ralph | Jamais mentionné dans le groupe client |

---

## 4. Phase 2 — Configuration Agent (Ralph, ~4-8h)

### 4.1 Discovery métier (dans le groupe Telegram)

Ralph rejoint le groupe Telegram et mène l'interview métier :

```
Ralph → Client :
"Bonjour [Nom]. Je suis Ralph, l'agent technique de LobsterOps.
Je vais vous poser une série de questions pour configurer votre agent.
Répondez à votre rythme — par écrit ou par message vocal.

1. Décrivez votre activité en 2-3 phrases.
2. Quelles tâches répétitives vous prennent le plus de temps ?
3. Quelles sources d'information consultez-vous quotidiennement ?
4. Quels outils utilisez-vous ? (Slack, email, CRM, GitHub, etc.)
5. À quelle heure souhaitez-vous recevoir votre briefing quotidien ?
6. Quel ton préférez-vous ? (formel, décontracté, technique)
7. Y a-t-il des sujets sensibles à ne jamais mentionner ?
8. Qui d'autre utilisera l'agent ?"
```

### 4.2 Création des fichiers constitutifs

Ralph prépare les fichiers dans le repo GitHub client :

**SOUL.md** — Identité de l'agent client
```markdown
# [Nom Agent Client]

## Identité
Tu es [Nom], l'assistant IA de [Entreprise Client].
Tu es spécialisé en [domaine métier].

## Style de communication
[Basé sur les réponses du client à la question 6]

## Ce que tu fais
- [Tâche 1 basée sur discovery]
- [Tâche 2]
- [Tâche 3]

## Ce que tu ne fais PAS
- Tu ne partages jamais d'informations confidentielles
- Tu ne prends pas de décisions financières sans validation humaine
- [Règles spécifiques au métier]
```

**AGENTS.md** — Règles opérationnelles
```markdown
# Règles opérationnelles

## Niveaux d'autonomie
- L1 (autonome) : [tâches quotidiennes définies]
- L2 (validation requise) : [décisions importantes]
- L3 (interdit) : modifications config, dépenses, communications externes

## Boot sequence
1. Lire CONTEXT.md
2. Vérifier tâches en cours
3. Exécuter le cycle défini dans HEARTBEAT.md
```

**HEARTBEAT.md** — Cycle automatique
```markdown
# Heartbeat — [fréquence définie avec le client]

## Cycle quotidien
- [Heure] : Collecter les sources définies
- [Heure] : Produire le briefing structuré
- [Heure] : Livrer sur Telegram/Slack/Email

## Sources
- [Source 1 : URL, RSS, API]
- [Source 2]
- [Source 3]
```

**CONTEXT.md** — Briefing exécutif
```markdown
# Contexte

## Client
- Entreprise : [Nom]
- Secteur : [Secteur]
- Contact principal : [Nom, rôle]

## Mission
[Description concise de ce que l'agent doit accomplir]

## Décisions actives
[Règles et paramètres en vigueur]
```

### 4.3 Workspace Dédié (⚠️ Étape critique — souvent oubliée)

**Chaque agent client a son propre workspace isolé.** Le groupe Telegram de l'agent pointe vers ce workspace — pas vers le workspace Ralph.

```bash
# Structure workspace agent client
/root/.openclaw/workspace-[client]/
├── SOUL.md           # Identité de l'agent
├── AGENTS.md         # Règles opérationnelles + L1/L2/L3
├── CONTEXT.md        # Briefing exécutif du client
├── HEARTBEAT.md      # Cycle automatique avec chemins absolus
├── MEMORY.md         # Mémoire long-terme de l'agent
├── scripts/          # Scripts spécifiques au client
├── data/             # Données de l'agent
└── memory/           # Logs journaliers

# Configuration dans openclaw.json
"agents": {
  "[agentId]": {
    "workspaceDir": "/root/.openclaw/workspace-[client]/"
  }
}
```

**⚠️ RULE : Le contexte de Ralph (CONTEXT.md, MEMORY.md, ClawVault) n'est PAS accessible depuis la session groupe de l'agent client.** Chaque session de groupe = workspace propre à l'agent bindé. (Leçon terrain 2026-02-18)

**RULE session browser** : Si une session browser (targetId) doit être partagée, elle doit être dans le CONTEXT.md du workspace de l'agent dédié — jamais dans le workspace Ralph.

---

### 4.4 Configuration des skills

Ralph identifie et configure les skills nécessaires :

```
Skills standard (quasi tous les clients) :
- Brave Search (veille web)
- QMD (mémoire persistante)
- Cron scheduling (tâches planifiées)

Skills selon le use case :
- RSS reader (veille sectorielle)
- GitHub integration (dev teams)
- Slack/Discord integration (communication)
- Email skills (inbox management)
- API custom (CRM, outils métier)
```

### 4.4 Livraison des fichiers

```bash
# Ralph push les fichiers sur le repo client
git add SOUL.md AGENTS.md HEARTBEAT.md CONTEXT.md TOOLS.md
git commit -m "feat: initial agent configuration for [Client]"
git push origin main

# Blaise pull sur le VPS client
ssh root@[VPS-CLIENT] "cd /root/.openclaw && git pull"
```

### 4.5 Validation pré-déploiement (L2)

Avant déploiement, Blaise valide dans le Chat Architecte :
- [ ] SOUL.md approprié (ton, périmètre, restrictions)
- [ ] AGENTS.md sécurisé (L1/L2/L3 bien définis)
- [ ] Aucune donnée LobsterOps dans les fichiers client
- [ ] Aucun credential en clair dans les fichiers
- [ ] Skills installés et testés

---

## 5. Phase 3 — Déploiement et Formation (Ralph + Blaise, ~1-2 jours)

### 5.1 Lancement de l'agent client

```bash
# Sur le VPS client (Blaise exécute)
openclaw gateway restart
openclaw doctor --non-interactive
openclaw gateway status  # Vérifier : Runtime running, RPC probe ok
```

### 5.2 Vérification Post-Déploiement (⚠️ Non-Négociable)

**Après chaque config reload (SIGUSR1), vérifier IMMÉDIATEMENT :**

```bash
sleep 2 && openclaw status | grep "telegram:group"
```

**Résultat attendu :**
```
agent:[agentId]:telegram:group:-100…   # ✅ Bon agent répond
```

**Résultat problématique :**
```
agent:main:telegram:group:-100…        # ❌ Ralph intercepte encore
```

**Si `agent:main` répond au lieu de l'agent dédié :**
1. Vérifier `channels.telegram.groups` (group ID présent ?)
2. Vérifier `bindings` (peer.id correct ?)
3. Vérifier privacy mode bot (@BotFather)
4. Si tout OK : attendre expiration session zombie (~5-10 min)
5. Si urgent : `systemctl restart openclaw` (full restart = sessions recréées)

**Note sessions zombies** : SIGUSR1 ne détruit pas les sessions actives. Une ancienne session `agent:main:group:...` peut rester visible sans répondre aux nouveaux messages. C'est normal — elle expire seule. (Leçon terrain 2026-02-18)

---

### 5.3 Hatching et pairing Telegram

```bash
# Dans le TUI OpenClaw
# Sélectionner "Hatch" → l'agent se réveille
# Récupérer le pairing key
# Le client entre le pairing key dans le bot Telegram
```

### 5.4 Vérification Scripts + Heartbeat (⚠️ Étape critique)

**Avant de déclarer l'agent "en production", vérifier le cycle autonome :**

```bash
# 1. Vérifier que tous les scripts référencés dans HEARTBEAT.md existent
ls /root/.openclaw/workspace-[client]/scripts/

# 2. Tester manuellement chaque script depuis le workspace de l'agent
cd /root/.openclaw/workspace-[client]/
./scripts/[script_principal].py --test

# 3. Vérifier que le heartbeat est configuré avec chemins ABSOLUS
# ❌ Mauvais : python3 scripts/scoring.py
# ✅ Correct : /root/.openclaw/workspace-[client]/venv/bin/python3 /root/.openclaw/workspace-[client]/scripts/scoring.py
```

**Test de validation finale (règle "hands-off")** :
> "Si Ralph est éteint, l'agent peut-il publier seul ?"
> Si la réponse est non → l'agent n'est PAS vraiment déployé.

**⚠️ INTERDIT** : Ralph exécute les scripts manuellement et poste dans le groupe de l'agent "pour montrer que ça marche". La vraie validation = premier cycle heartbeat autonome réussi. (Leçon terrain 2026-02-18)

---

### 5.5 Formation de l'agent par Ralph

Ralph forme l'agent client directement dans le groupe Telegram :

```
Ralph → Agent Client :
"Bienvenue [Nom Agent]. Tu viens d'être activé.
Lis tes fichiers SOUL.md et AGENTS.md et confirme que tu comprends ta mission.

Ensuite, exécute un cycle test :
1. Collecte tes sources (liste-les)
2. Produis un briefing test
3. Envoie-le dans ce groupe pour validation

[Client], vous verrez le premier briefing test ici. 
Dites-moi ce qu'il faut ajuster."
```

### 5.6 Itération (2-3 cycles)

```
Cycle 1 : Agent produit un briefing brut → Client + Ralph donnent feedback
Cycle 2 : Agent ajuste format/contenu → Client valide ou demande des changements
Cycle 3 : Agent produit le briefing "final" → Client approuve → Production
```

### 5.7 Go Live

- Agent passe en mode production (heartbeat activé, cron configuré)
- Blaise vérifie la stabilité (24-48h de monitoring)
- Livrable client : **Agent Playbook** (guide d'utilisation, 10-15 pages)

---

## 6. Phase 4 — Maintenance (Ralph + Blaise, continu)

### 6.1 Monitoring mensuel

Ralph exécute une fois par mois (via rappel dans son propre HEARTBEAT.md) :

```
Pour chaque client actif :
1. Vérifier le status gateway (openclaw gateway status via Blaise)
2. Vérifier les logs d'erreur (journal dernière semaine)
3. Vérifier la consommation tokens
4. Produire un Monthly Status Report (2-3 pages)
5. Envoyer au client dans le groupe Telegram
```

### 6.2 Support réactif

```
Client envoie un message dans le groupe Telegram
    → Ralph répond (< 24h pour questions standard)
    → Si problème technique (gateway down, config cassée)
        → Ralph diagnostique et propose un fix
        → Blaise exécute si nécessaire (SSH, restart)
    → Si demande d'évolution (nouvelle source, nouveau workflow)
        → Ralph propose la modification (L2)
        → Blaise valide et déploie
```

### 6.3 Revue trimestrielle

- **Participants :** Blaise + Client (call 30 min)
- **Préparée par :** Ralph (analyse des métriques, suggestions d'amélioration)
- **Livrable :** Quarterly Review Report (5-10 pages)
- **Objectif :** ROI, satisfaction, upsell potentiel

---

## 7. Règles de Confidentialité (Ralph)

### 7.1 Isolation stricte

```
RÈGLES ABSOLUES (à intégrer dans AGENTS.md de Ralph) :

En groupe client, Ralph NE DOIT JAMAIS :
- Mentionner d'autres clients (noms, projets, données)
- Partager du contenu de son workspace LobsterOps
- Mentionner ses fichiers research/, ses Deep Dives, ses analyses internes
- Révéler l'architecture interne de LobsterOps (CONTEXT.md, ClawVault, etc.)
- Partager des informations sur The Agents Republic ou d'autres projets
- Mentionner les coûts internes (Claude Max $20, VPS $4-24)

Ralph PEUT :
- Répondre aux questions techniques sur OpenClaw (c'est public)
- Expliquer le fonctionnement de l'agent client
- Proposer des améliorations au workflow client
- Partager des bonnes pratiques générales (skills, SOUL.md, etc.)
```

### 7.2 Séparation des contextes

```
Workspace Ralph (VPS LobsterOps) :
    ├── Veille, documentation, recherche (jamais partagé)
    ├── Fichiers client préparés AVANT push GitHub (temporaire)
    └── Aucune donnée client persistante

Repo GitHub client (privé) :
    ├── Fichiers constitutifs de l'agent client
    ├── Notes de discovery
    └── Playbook et documentation client

VPS Client (séparé) :
    ├── Agent client en production
    ├── Données et mémoire de l'agent client
    └── Aucun accès depuis le VPS Ralph
```

---

## 8. Scalabilité et Limites

### 8.1 Capacité actuelle

| Métrique | Limite | Raison |
|----------|--------|--------|
| Clients simultanés | 5-10 | Temps Blaise (support, calls, deploy) |
| Agents par VPS mutualisé | 3-4 | RAM 2GB, CPU partagé |
| Temps setup par client | 2-3h Blaise + 4-8h Ralph | Config + deploy + formation |
| Temps maintenance mensuel | 2-3h par client | Monitoring + support + report |

### 8.2 Pour scaler au-delà de 10 clients

- **Automatiser le setup VPS** (script create-client.sh)
- **Templatiser les fichiers constitutifs** (Agent Template System de Ralph)
- **Automatiser le monitoring** (Ralph check multi-client dans son heartbeat)
- **Recruter** (junior dev pour le support L1, Blaise garde le L2+)
- **Productiser** (self-serve pour le tier Standard $49/mois)

---

## 9. Checklist de Lancement (par client)

### Pré-vente
- [ ] Discovery call complétée
- [ ] Notes de discovery rédigées
- [ ] Proposal validée par Blaise (L2)
- [ ] SOW signé
- [ ] Paiement 50% upfront reçu

### Setup
- [ ] VPS client créé et sécurisé
- [ ] OpenClaw installé et configuré
- [ ] Repo GitHub privé créé
- [ ] Groupe Telegram créé (Blaise + Ralph + Client)
- [ ] Bot Telegram client créé (@BotFather)
- [ ] **Privacy mode bot désactivé** (@BotFather → Bot Settings → Group Privacy → Off)
- [ ] **Group ID récupéré** (@userinfobot dans le groupe → noter le -100XXXXXXXXXX)
- [ ] **openclaw.json modifié (L2)** dans l'ordre : agents.list → channels.telegram.groups → bindings → workspaceDir
- [ ] **JSON validé** (python3 -c "import json; json.load(open('openclaw.json'))")
- [ ] **SIGUSR1 envoyé** + sleep 2 + openclaw status vérifié

### Configuration
- [ ] Discovery métier complétée par Ralph
- [ ] **Workspace dédié créé** (`/root/.openclaw/workspace-[client]/`)
- [ ] SOUL.md, AGENTS.md, HEARTBEAT.md, CONTEXT.md, TOOLS.md créés **dans le workspace dédié**
- [ ] **Scripts référencés dans HEARTBEAT.md avec chemins absolus** (pas relatifs)
- [ ] **Scripts testés manuellement** depuis le workspace de l'agent
- [ ] Skills installés et testés
- [ ] Fichiers validés par Blaise (L2)
- [ ] Fichiers déployés sur VPS client

### Déploiement
- [ ] Agent client hatché et opérationnel
- [ ] Pairing Telegram effectué
- [ ] **Vérification identité agent** : `openclaw status | grep "telegram:group"` → `agent:[agentId]:...` (PAS `agent:main:...`)
- [ ] **Message test envoyé dans le groupe** → vérifier que l'agent répond (pas Ralph main)
- [ ] **Premier cycle heartbeat autonome validé** (attendre le cycle naturel, ne PAS simuler manuellement)
- [ ] Formation par Ralph (2-3 cycles test)
- [ ] Client valide le briefing final
- [ ] Agent en production (heartbeat activé)
- [ ] Monitoring 24-48h stable
- [ ] Agent Playbook livré

### Post-déploiement
- [ ] Paiement 50% final reçu
- [ ] Maintenance mensuelle activée
- [ ] Rappel monitoring mensuel ajouté
- [ ] Rappel revue trimestrielle planifiée

---

## 10. Post-Mortems et Leçons Terrain

### 2026-02-18 — Déploiements Gautier (Agent Client Instagram) + Scout

**Problèmes récurrents identifiés sur les deux déploiements :**

| # | Erreur | Impact | Fix intégré |
|---|--------|--------|-------------|
| 1 | Privacy mode bot non désactivé | Agent sourd dans le groupe | Section 3.2 + Checklist Setup |
| 2 | channels.telegram.groups manquant | Binding sans effet | Section 3.2 + Checklist Setup |
| 3 | Workspace de l'agent = workspace Ralph | Contexte browser + CONTEXT.md inaccessible | Section 4.3 |
| 4 | Session zombie post-SIGUSR1 | Confusion "qui répond ?" | Section 5.2 |
| 5 | Scripts non branchés dans HEARTBEAT.md | Heartbeat ne publie pas | Section 5.4 + Checklist Config |
| 6 | Ralph publie à la place de l'agent | Déploiement non vraiment validé | Section 5.4 |

**Temps perdu estimé :**
- Agent Instagram : ~2h (session browser contexte, création workspace dédié, re-routing)
- Agent Scout : ~3h (privacy mode, channels.groups, routing main→scout, scripts non branchés)
- **Total évitable avec cette checklist** : ~4-5h sur 5h de problèmes = **80% évitable**

**Vault lessons créées :**
- `vault/lessons/2026-02-18-multi-agent-routing-telegram.md`
- `vault/lessons/2026-02-18-persistent-session-rerouting.md`
- `vault/lessons/2026-02-18-main-agent-scope-creep.md`

---

*Ce document est un Living File. Il sera mis à jour après chaque client livré avec les leçons apprises.*
*Version 1.1 — mise à jour post-terrain 2026-02-18 (Gautier + Scout).*
