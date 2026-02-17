# Code Factory: Harness Engineering Pattern
**Source:** Ryan Carson (@ryancarson) — X post, 14 Feb 2026  
**Saved:** 2026-02-17  
**Tags:** coding-agent, CI/CD, multi-agent, review-automation, skills-deployment

---

## TL;DR

Pattern de repo pour boucle autonome agent → review → remediate → merge.  
Codex écrit le code, un review agent (Greptile/CodeRabbit/CodeQL) valide, un risk gate contrôle les merges.

---

## Core Pattern (7 étapes)

1. **Risk contract machine-readable** — JSON central définissant risk tiers par path + required checks par tier
2. **Preflight gate avant CI** — `risk-policy-gate` tourne en premier, avant les jobs coûteux
3. **SHA discipline** — review state valide uniquement sur le commit courant (jamais de stale evidence)
4. **Single rerun-comment writer** — dédupe par marker + sha pour éviter les race conditions
5. **Remediation loop** — coding agent lit les findings → patch → push → rerun (sans bypass des gates)
6. **Auto-resolve bot threads** — uniquement après clean rerun, jamais les threads humains
7. **Browser evidence** — artefacts CI pour les flows UI, pas juste des screenshots dans la PR

---

## Risk Contract (structure JSON)

```json
{
  "version": "1",
  "riskTierRules": {
    "high": ["app/api/**", "lib/tools/**", "db/schema.ts"],
    "low": ["**"]
  },
  "mergePolicy": {
    "high": { "requiredChecks": ["risk-policy-gate", "harness-smoke", "Browser Evidence", "CI Pipeline"] },
    "low": { "requiredChecks": ["risk-policy-gate", "CI Pipeline"] }
  }
}
```

---

## Leçons clés

- **SHA matching non-négociable** : merger sur du "clean" stale = faille
- **Ordering déterministe** : preflight gate avant CI fanout
- **Un seul canonical rerun writer** par repo
- **Incidents → harness cases** : pas de fix one-off, croissance de la couverture long terme

---

## Stack de référence (implémentation Ryan Carson)

- Review agent : Greptile
- Remediation agent : Codex Action
- Workflows : `greptile-rerun.yml`, `risk-policy-gate.yml`, `greptile-auto-resolve-threads.yml`

---

## Applicabilité LobsterOps

**Maintenant** : rien — pas de repo de code actif avec CI/CD  
**Quand on code des skills** : 
- `risk-policy.json` pour les skills (high = accès réseau/filesystem, low = markdown only)
- Preflight gate avant déploiement skill en prod
- SHA discipline pour config updates

**Référence à relire lors de** : Phase 3 Researcher agent, création de skills custom, tout pipeline CI pour skills OpenClaw.

---

## Lien inspiration mentionné

Blog post par @_lopopolo (non archivé ici)
