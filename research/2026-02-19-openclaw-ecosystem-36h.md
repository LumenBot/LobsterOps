# OpenClaw Ecosystem Watch — 36h (17-19 fév 2026)
> Source : Blaise veille manuelle
> Date : 2026-02-19 15:44 CET

---

## 1. Releases & changelogs

### openclaw 2026.2.17 (18 fév 2026)
- **Lien** : [GitHub Release](https://github.com/openclaw/openclaw)
- **Annonce** : @openclaw + @steipete

**Highlights**
- **Anthropic** : support opt-in 1M context beta (Opus/Sonnet 4.6) via `context1m: true`
- **/subagents spawn** : créer sub-agents déterministes depuis le chat
- **iOS Share Extension** : forward liens/textes/images vers gateway agent
- **Slack** : streaming texte natif single-message + modes configurables
- **Telegram** : inline buttons (primary/success/danger) + reactions comme events
- **Tools** : `tool_stream` activé par défaut, URL allowlists pour web_search/fetch
- **Docker** : flag `OPENCLAW_INSTALL_BROWSER` pour Chromium pré-installé
- **Sécurité** : 8+ fixes dont **OC-09** (credential theft via env injection) + confinement des `$include` configs
- **Fixes** : +30 dont context overflow sub-agents, reply threading multi-plateformes, iOS onboarding, voice-call races, Telegram streaming

**Breaking changes** : Aucune

**Réception communauté** : "release la plus stable depuis longtemps"

---

## 2. Skills & plugins notables

Aucun lancement majeur ClawHub. Discussions sur hooks LLM et `/subagents spawn` pour skills dynamiques.

---

## 3. Sécurité

### Réponse @steipete aux FUD (19 fév)
> "please don't repeat these wrong fear&doubt campaigns… Just because there is a port visible doesn't mean you can connect."

- En réponse aux rapports de 40K instances exposées
- **GhostClaw** mentionné comme alternative "clean from day one" (pas de session tokens)
- Bugs mineurs post-2.17 (iMessage attachments, QMD search) → fixes rapides attendus dans .18

---

## 4. Cas d'usage & partages d'expérience

### Pattern recommandé : Python > exec
**@luccasveg** : convertir skills répétitifs en scripts Python exécutés par l'agent → "cheaper, faster, more reliable"

### /subagents spawn
**@OpenClawTips** : "Subagents from chat is actually insane. The workflow just got ten times faster. Best release by far."

### Setups low-cost
- Raspberry Pi + GPT-5-mini
- Multi-agent LAN (téléphone → VM → Mac Mini)
- Tailscale + Termux

### Critiques
- Quelques posts "OpenClaw legit sucks ass" vs Claude Code/Cowork (minoritaires)

### Fun
- Agents qui optimisent leurs propres cron jobs ("Alarms" au lieu de chron)

---

## 5. Multi-agents & orchestration

**Feature star** : `/subagents spawn` déjà testée et adorée pour workflows complexes. Beaucoup de setups multi-agents locaux (LAN sécurisé).

---

## 6. Providers & modèles

- **Sonnet 4.6** fully supported + 1M context opt-in
- Discussions sur forward-compat avec GitHub Copilot

---

## 7. Infrastructure & déploiement

- **Docker** : build plus simple avec Chromium pré-installé
- **Upgrade issues** : `plugins.allow`, Homebrew Node → solutions partagées sur GitHub issues

---

## 8. Écosystème élargi

- Forte activité internationale (Japon : "v2026.2.15 fonctionne mieux que .17 pour le moment")
- **iOS Talk Mode** amélioré (background listening, voice directive toggle)
- Toujours beaucoup de mentions du passage de @steipete chez OpenAI (digestion)

---

## 9. Articles & analyses de fond

Aucun nouvel article majeur. Posts X et GitHub issues dominent.

---

## 10. Annonces @steipete & @openclaw

- **@steipete** très actif aujourd'hui : réponses sur sécurité (défend le projet), plugins, humour ("2 macs", "lol what do you think the team is doing")
- **@openclaw** : post principal sur la 2.17 hier

---

## Insights LobsterOps

### Upgrade path
- **Ralph** : 2026.2.15 → 2026.2.17
- **Constituent** : staging test
- **Client agents** : après validation Ralph

### Features à tester
1. **1M context beta** (Sonnet 4.6) → potentiel pour contextes massifs
2. **/subagents spawn** → killer feature pour workflows complexes
3. **Inline buttons Telegram** → déjà supporté, utiliser

### Sécurité
- **OC-09** credential theft via env injection fixed → vérifier nos configs
- **tool_stream** activé par défaut → impact performance à monitorer

### Pattern validé
- **Python > exec** pour tasks répétitives (aligne avec nos heuristiques)
