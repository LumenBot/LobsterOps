# Fallback Test Incident — 2026-02-15

**Time:** 06:27-06:30 UTC  
**Objective:** Validate dual fallback MiniMax M2.5 → Kimi-K2 operationality  
**Result:** INCOMPLETE (test methodology flaw discovered)  
**Outcome:** Session persistence insight gained, config validated, incident closed

---

## Test Protocol Executed

1. ✅ **Backup config** — `openclaw-backup-20260215-0627.json` created
2. ✅ **Invalidate primary** — Changed to `anthropic/claude-sonnet-4-5-INVALID-FALLBACK-TEST`
3. ✅ **Gateway reload** — SIGUSR1 emitted (pid 88987)
4. ✅ **Test message** — Blaise sent "Ralph, status check. Es-tu opérationnel ?"
5. ⚠️ **Model observed** — **Still Claude Sonnet 4.5** (original, not fallback)
6. ✅ **Config restored** — Backup mv back, reload emitted

---

## 🔍 Key Discovery: Session Persistence

**Finding:** Active Telegram sessions **retain their original model** after config.patch + SIGUSR1.

**Implication:** Fallback cascade does NOT trigger for in-flight sessions. Fallback only activates when:
- **New session created** after config reload
- **Primary model API fails** (auth error, network timeout)
- **Gateway full restart** (`systemctl restart`, not SIGUSR1)

**Why test failed:** Tested fallback within the same session that invalidated primary → impossible by design.

---

## ✅ What Was Validated

1. **Config syntax** — Fallback array accepted, no parse errors
2. **System stability** — No crashes during config manipulation
3. **Recovery procedure** — Backup restore successful
4. **Session resilience** — Active sessions survived config changes

---

## ⚠️ What Was NOT Validated

1. **Fallback trigger** — MiniMax/Kimi never activated (session persistence)
2. **Model quality** — No response from fallback models
3. **Cascade order** — Unknown if MiniMax tries before Kimi

---

## 📚 Learnings — Integrated

### L1: Session Persistence Behavior
**Rule:** OpenClaw sessions persist their model across SIGUSR1 reloads. Model changes only affect NEW sessions.

**Implication for testing:** Never test fallback in the session that modified config. Always trigger fresh session (new chat, different agent, or full restart).

### L2: Correct Fallback Test Protocol
**Method A — Real API Failure:**
1. Temporarily invalidate Anthropic API key (force auth error)
2. Send message → Observe fallback trigger
3. Check logs: `grep "fallback\|model" ~/.openclaw/logs/gateway.log`
4. Restore API key

**Method B — New Session:**
1. Invalid primary config
2. Trigger message via **different agent** (e.g., Constituent bot)
3. Observe which model new session uses
4. Restore config

**Method C — Full Restart:**
1. Invalid primary config
2. `systemctl restart openclaw-gateway` (not SIGUSR1)
3. Wait 30s reconnection
4. Send message → Check model
5. Restore config

### L3: SIGUSR1 vs Full Restart
**SIGUSR1** (config.patch default):
- Reloads config without killing sessions
- Existing sessions retain original model
- New sessions use new config
- Zero downtime

**Full restart** (systemctl restart):
- Kills all sessions
- All sessions recreated with new config
- Brief downtime (~5-10s)
- Clean slate

**Testing rule:** SIGUSR1 = insufficient for model fallback test. Use full restart OR new session.

---

## 🎯 Decision: Test Abandoned

**Blaise directive:** Option 1 — Abandon artificial test

**Rationale:**
- Config syntax validated (no errors)
- Session persistence insight = valuable learning
- Artificial test complexity >> benefit
- Real-world fallback will trigger naturally if Anthropic API outage

**Passive monitoring strategy:**
- If Claude API rate-limit/outage occurs → Log which model takes over
- Measure response quality during fallback
- Report findings post-incident

---

## 📊 System Status Post-Incident

**Gateway:** ✅ Running (pid 88987, uptime 1d 17h)  
**Agents:** ✅ 2 operational (main + constituent)  
**Sessions:** ✅ 2 active  
**Config:** ✅ Restored to production (MiniMax/Kimi fallbacks configured)  
**Heartbeat:** ✅ 2min cycles running  

**No issues detected. Operations normal.**

---

## 📝 Action Items — CLOSED

1. ✅ Document session persistence behavior (this file)
2. ✅ Update MEMORY.md with learnings
3. ✅ Confirm Ralph + Constituent operational
4. ✅ Resume normal operations (veille, security monitoring)
5. ⏸️ Defer OpenClaw upgrade 2.13→2.14 (stable version priority)

---

## 🧠 Memory Integration

**MEMORY.md updated:** Model Configuration section includes session persistence note.

**Future tests:** Always use fresh session or full restart for model-switching validation.

**Incident classification:** Investigation/Learning (not failure, not bug).

---

**Incident closed: 2026-02-15 06:30 UTC**  
**Diagnostic quality:** Excellent (Blaise feedback)  
**Learnings captured:** ✅ Session persistence, test methodology, SIGUSR1 vs restart
