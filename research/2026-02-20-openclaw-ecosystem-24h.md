# OpenClaw Ecosystem Watch — 24h (19-20 fév 2026)
> Source : Blaise veille manuelle
> Date : 2026-02-20 15:00 CET

---

## 1. Releases & changelogs

### openclaw 2026.2.19 (et beta.1)
**Date** : 19 février 2026 (16:21 puis 17:35 UTC)
**Lien** : GitHub Release | Thread @openclaw

**Nouveautés phares**
- **Apple Watch MVP** complet (inbox UI, notification relay, gateway commands, APNs wake + auto-reconnect)
- **Gateway** : auth par défaut token + device management + paired-device hygiene
- **OTEL v2** migration complète + plugin/hooks hardening
- **40+ security hardening fixes** (SSRF, exec, token auth, sandboxing, voice-call prototype-pollution, Windows daemon injection, etc.)
- **Dashboard** : nudge automatique pour updater
- **Fixes** : streaming partiel, iOS crashes, Telegram unification, cron heartbeats

**Breaking changes** : Aucune

**Réception communauté** : "biggest hardening drop yet" + "infrastructure getting serious"

---

## 2. Skills & plugins notables

Aucun lancement ClawHub. Discussions sur hooks LLM et Apple Watch gateway pour futurs skills watch-specific.

---

## 3. Sécurité

- 40+ fixes applaudis ("biggest hardening drop yet")
- FUD récurrent sur 135k+ instances exposées, mais release durcit tout par défaut (token auth, no_auth audit)
- Aucune nouvelle CVE ni advisory dans les 24h

---

## 4. Cas d'usage & partages d'expérience

### Highlights
- **@CryptoEights** : "Now my openclaw is training to become on-chain analyst" (analyse MicroStrategy + report)
- **@AmirAnonn** (persan) : prompt AGENTS.md ultra-puissant pour "figure it out" sans questions inutiles
- **@nusionx** : optimisation système prompt pour éviter TPM limits (30k+ caractères trop lourds)
- **@agentnativedev** : article OpenClaw variants sur hardware 10$/10MB RAM
- **@Gorden_Sun** : comparaison Opus 4.5 vs OpenClaw ("rien de différent")
- **@CryptoCorvus1** : Lobster qui essaie de le gaslight (screenshot drôle)

### Setups
- Plusieurs low-cost (RPi, vieux Mac, Termux) + tips mémoire

---

## 5. Multi-agents & orchestration

Pas de nouvelle feature, mais beaucoup de retours sur nested sub-agents et /subagents spawn (release précédent), utilisés en prod avec Apple Watch relay.

---

## 6. Providers & modèles

Mentions fréquentes Opus 4.5/4.6 et GLM-5 dans setups communautaires. Pas de nouveau provider.

---

## 7. Infrastructure & déploiement

- **Shoutout @useblacksmith** : sponsoring toute l'infrastructure de build (tests automatiques chaque commit)
- Plusieurs partages setups Apple Watch + iOS + gateway
- Docker/CLI : facilité avec nouveau APNs et device management

---

## 8. Écosystème élargi

- Forte activité internationale (Chine : comparaison projet médical ; Iran/Persan : prompts avancés)
- Sponsoring visible : Blacksmith paye les CI
- **Hype Apple Watch MVP** : premiers tests et screenshots aujourd'hui

---

## 9. Articles & analyses de fond

Aucun nouvel article publié 24h. Vidéos récapitulatives (Julian Goldie SEO, etc.) sur release 2.19 circulent.

---

## 10. Annonces @steipete & @openclaw

**@openclaw** : post principal hier sur 2.19 + shoutout sponsor

**@steipete** très actif (20 fév) :
- Réponses sur dépendances à déplacer en plugins
- Humour sur "every day I learn new things on the internet as to what I apparently did"
- Commentaires sur vulnérabilités ("might have overshot on one report") et subs mensuels
- Réponse crypto-only
