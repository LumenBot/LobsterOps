# ðŸ¦ž OpenClaw Knowledge Index â€” Community Edition

> **Role:** Quickly find any information in the community documentation.
> Organized by theme, with pointers to precise sections in each document.
>
> **Version:** 1.1-community | **Last updated:** February 12, 2026

---

## Document Map

| Abbrev | Document | Role | Est. Lines |
|--------|----------|------|------------|
| **PROJ** | `OpenClaw_Expert_Agent_Project.md` | Project vision, roadmap, tokenomics | ~200 |
| **ENC** | `OpenClaw_Encyclopedia.md` | Encyclopedia â€” "what is it?" | ~890 |
| **PB** | `OpenClaw_Operators_Playbook.md` | Operational guide â€” "what do I do?" | ~560 |
| **TD** | `OpenClaw_Technical_Deep_Dives.md` | In-depth technical annexes | ~1,800 |
| **EW** | `OpenClaw_Ecosystem_Watch.md` | Releases, articles, signals | ~125 |
| **WSL** | `OpenClaw_Setup_Guide_WSL2.md` | Windows step-by-step install | ~440 |
| **CTB** | `CONTRIBUTING.md` | How to contribute | ~200 |

---

## ðŸ—ï¸ ARCHITECTURE & CONCEPTS

| Topic | Where to Look |
|-------|--------------|
| 6-stage execution pipeline | ENC Â§2.1 |
| Lane Queue System | ENC Â§2.2 |
| Semantic Snapshots (web navigation) | ENC Â§2.3, TD Annex H |
| Memory system | ENC Â§2.4, TD Annex G (RAG), TD Annex J (compaction) |
| Configuration files (SOUL.md, MEMORY.md, etc.) | ENC Â§2.5, PB Phase 2 |
| Multi-layer security | ENC Â§2.6, TD Annex M |
| Living Files vs Dead Files | ENC Â§10.4, TD Annex T.2, PROJ Â§3.3 |
| Feedback loops & self-improving agents | ENC Â§10.4, TD Annex U |
| "Autopilot Era" (copilot to autonomous) | ENC §10.4 |
| Commercial ecosystem emergence ($225K exit) | ENC §10.4 |
| Lex Fridman podcast (steipete) | EW Week Feb 10-12 |
| SKILL.md standard convergence (OpenAI+Anthropic+OpenClaw) | ENC §4.1, TD Annex W |
| GLM-5 (744B, MIT, frontier open-source) | ENC §8.2, EW Week Feb 10-12 |
| Kimi Agent Swarm (100 sub-agents) | EW Week Feb 10-12 |
| OpenAI Responses API agentic primitives | EW Week Feb 10-12 |

## ðŸ¤– MULTI-AGENTS

| Topic | Where to Look |
|-------|--------------|
| Multi-agent routing concept | ENC Â§3.1 |
| AGENTS.md configuration | ENC Â§3.2, PB Phase 5 |
| Routing rules | ENC Â§3.3 |
| Documented multi-agent patterns | ENC Â§3.5 |
| OpenclawInterSystem (OIS) | ENC Â§3.6 |
| 8-agent blueprint (ScreenSnap Pro) | TD Annex S (complete) |
| Claim locking (race conditions) | TD Annex S.5, PB Phase 5 |
| Quality gates (Sage pattern) | TD Annex S.4 |
| PM agent self-healing | TD Annex S.6 |
| Collective intelligence (bottleneck) | TD Annex P |
| Roundtable voting (consensus) | TD Annex K |
| Multi-agent dispatch + chairman | TD Annex T.3 |
| Skill stacking (pipeline) | TD Annex U.4 |
| Battle-tested 24/7 patterns (memory split, crash recovery, trust tiers) | TD Annex V |
| Security model tiering (Opus external, Sonnet internal) | TD Annex V.5, PB Cheat Sheet |
| Daily Synthesis Loop (self-improving agent) | TD Annex V.7 |
| SKILL.md standard convergence | TD Annex W, ENC §4.1 |
| Stripe Minions (enterprise pattern) | TD Annex O |

## ðŸ§© SKILLS & PLUGINS

| Topic | Where to Look |
|-------|--------------|
| Skill architecture | ENC Â§4.1 |
| ClawHub registry | ENC Â§4.2 |
| Skill catalog by category | ENC Â§4.3 |
| Creating custom skills | ENC Â§4.4, PB Phase 4 |
| Malicious skills / security | ENC Â§7.2, EW Security section |
| Skill stacking pattern | TD Annex U.4 |

## ðŸ” SECURITY

| Topic | Where to Look |
|-------|--------------|
| CVE-2026-25253 | ENC Â§7.1 |
| Complete threat list | ENC Â§7.2 |
| Security checklist | ENC Â§7.3, PB Phase 3 |
| Security tools | ENC Â§7.4 |
| Reference analyses (CrowdStrike, Vectra, etc.) | ENC Â§7.5 |
| SHIELD.md standard | TD Annex M.2 |
| openclaw-shield (Knostic) | TD Annex M.3 |
| Pointbreak pattern (pragmatic hardening) | ENC Â§6.6, PB Â§3.4 |
| Data Gravity concept (Cyera) | ENC §10.4 |
| Kaspersky / SOCRadar analyses | ENC §7.5, EW Week Feb 10-12 |
| Google OAuth banning risk | EW Week Feb 10-12 |

## ðŸ’° COSTS & OPTIMIZATION

| Topic | Where to Look |
|-------|--------------|
| Cost problem explained | ENC Â§8.1 |
| Model tiering strategy | ENC Â§8.2, PB Â§0.3 |
| Cost reduction tips | ENC Â§8.3 |
| ClawRouter / Claw Compactor | ENC Â§8.4, TD Annex Q |
| Opus 4.6 token hunger | ENC Â§8.2, EW Community signals |

## ðŸš€ DEPLOYMENT

| Topic | Where to Look |
|-------|--------------|
| Deployment matrix (14 methods) | ENC Â§6.1 |
| VPS comparison (6 providers) | ENC Â§6.1bis, TD Annex T.1 |
| 5-tier deployment ecosystem (25+ platforms) | ENC §6.1ter |
| One-click deployers (EasyClaw, Claw Cloud, etc.) | ENC §6.1ter |
| Multi-agent orchestration platforms (MissionControlAI) | ENC §6.1ter |
| Docker secure config | ENC Â§6.2, PB Â§1.3 |
| Installation & onboarding | ENC Â§6.3, PB Phase 1 |
| WSL2 specific | WSL (dedicated guide) |
| Network exposure | ENC Â§6.5, PB Â§3.2 |

## ðŸ“¦ ECOSYSTEM PROJECTS

| Topic | Where to Look |
|-------|--------------|
| Antfarm (deterministic teams) | ENC Â§5.1 |
| VoxYZ (autonomous loop) | ENC Â§5.2 |
| Lobster (workflow shell) | ENC Â§5.3, TD Annex A |
| Full ecosystem project table | ENC Â§5.4 |
| MCP integration | TD Annex B |
| Voice capabilities | TD Annex C |
| Framework comparison (vs CrewAI, LangGraph, etc.) | TD Annex D |

## ðŸ“ SOUL.md & AGENT IDENTITY

| Topic | Where to Look |
|-------|--------------|
| SOUL.md basics | PB Â§2.2 |
| Advanced templates | TD Annex E |
| Dynamic identity (soul-evil hook) | TD Annex E.5 |
| Multi-persona via multi-agents | TD Annex E.6 |
| Persona creation tools | TD Annex E.7 |
| Role Card 6-layer architecture (VoXYZ) | TD Annex E.8 |
| hardBans > Skills principle | TD Annex E.8 |
| Affinity Matrix (inter-agent relationships) | TD Annex E.9 |
| Memory-driven personality evolution | TD Annex E.10 |
| RPG stats as agent observability | TD Annex E.11 |
| Team-scale deployment (50 teammates) | ENC §9.3, EW Week Feb 10-12 |

## ðŸ¢ THIS PROJECT

| Topic | Where to Look |
|-------|--------------|
| Vision & architecture | PROJ Â§1-3 |
| Knowledge corpus contents | PROJ Â§4 |
| JUNO tokenomics | PROJ Â§5 |
| Roadmap | PROJ Â§6 |
| How to contribute | CTB |

---

*OpenClaw Knowledge Index — Community Edition v1.1. February 2026.*
