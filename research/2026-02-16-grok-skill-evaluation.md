# Grok Skill Evaluation — OpenClaw Integration Research

**Date**: 2026-02-16 09:29 UTC  
**Signal**: Christopher Stanley (@cstanley) reported Grok integration for OpenClaw  
**Source**: Blaise update (X post reference)  
**Status**: 🔍 INVESTIGATING

---

## 📋 Signal Summary

**Reported capabilities**:
- Real-time web searches via xAI Grok API
- X (Twitter) searches
- Citations + domain/handle filters
- Image/video understanding

**Installation**: `clawhub install grok` (reported)  
**Functions**: `search_web()`, `search_x()` (reported)  
**Requirements**: xAI API key

---

## 🔍 Investigation Findings

### Skill Availability

**Status**: ❓ **NOT CONFIRMED**

1. **ClawHub command**: ❌ Not found (`clawhub: command not found`)
   - Possible future feature or different command syntax
   - May be community tool vs official OpenClaw CLI

2. **Installed skills**: ❌ Grok skill not installed
   - `openclaw skills list` shows no grok skill
   - Not in default skills directory (`/usr/lib/node_modules/openclaw/skills/`)

3. **Christopher Stanley post**: ⏳ Not verified
   - X post URL provided: https://x.com/cstanley/status/2022900344003661988
   - Unable to verify via web search (Brave rate limited: 49/2000 quota used)
   - Need direct verification

### xAI Grok API Research

**Status**: ✅ **CONFIRMED — API EXISTS**

**Source**: Official xAI docs (https://docs.x.ai)

#### Pricing

**Token-based pricing**:
- **Starting from $0.20/M tokens** (competitive vs OpenAI/Anthropic)
- **Free credits**: Up to $175/month available
- **Token categories**:
  - Prompt tokens: Standard rate
  - Cached tokens: Reduced rate (prefix matching, same prompts)
  - Completion tokens: Standard rate
  - Reasoning tokens: Completion rate
  - Image tokens: 256-1792 per image (size-dependent)

**Cost breakdown** (from xAI official docs):

**Models** (recommended for OpenClaw use):
- **grok-4-1-fast-reasoning**: $0.20 input ($0.05 cached) / $0.50 output per 1M tokens
  - Context: 2M tokens, Rate: 4M TPM, 480 RPM
  - Capabilities: text + image → text, functions, structured, reasoning
- **grok-3-mini**: $0.30 input ($0.07 cached) / $0.50 output per 1M tokens
  - Context: 131K tokens, Rate: 480 RPM
  - Capabilities: text → text, functions, structured, reasoning

**Server-side tools** (KEY for veille use cases):
- **web_search**: $5 per 1,000 calls ($0.005 per call)
  - Search internet + browse web pages
- **x_search**: ~$5 per 1,000 calls (likely, price not fully shown)
  - Search X posts, user profiles, threads

**Comparison**:
- xAI Grok: $0.20/M input (cached $0.05), $0.50/M output
- Anthropic Haiku: $0.25/M input, $1.25/M output
- OpenAI GPT-4o-mini: $0.15/M input, $0.60/M output

#### Rate Limits

**Per-tier limits**:
- Max requests per minute (RPM)
- Max tokens per minute (TPM)
- Error code `429` when limit exceeded

**Mitigation**:
- Upgrade team tier
- Reduce request frequency
- Email support@x.ai for higher limits

**Cache optimization**:
- Prefix matching for repeated prompts
- `x-grok-conv-id: <uuid4>` header for cache hit likelihood
- Reduced cost for cached tokens

#### Features

**Real-time capabilities** (from Metronome pricing index):
- Real-time web search
- X/Twitter data integration
- DeepSearch for complex research queries

**Models available**:
- Grok 3 (latest)
- Grok 4 variants (grok-4, grok-4-1-fast-reasoning)
- Aurora image generation

**Context windows**:
- Varies by model (check Models and Pricing page)
- Image + text tokens must fit within context

---

## 🎯 Relevance Assessment

### For Ralph (LobsterOps Veille)

**Potential benefits**:
- ✅ **Real-time web searches** (vs Brave delayed indexing)
- ✅ **Native X monitoring** (Twitter @TheConstituent_, crypto × AI signals)
- ✅ **Citations + filters** (quality sources, domain/handle filtering)
- ✅ **Cost-effective** ($0.20/M tokens + $175/month free credits)
- ✅ **Cache optimization** (repeated queries at reduced rate)

**Trade-offs**:
- ⚠️ **xAI API dependency** (external service, cost, rate limits)
- ⚠️ **Privacy concerns** (queries logged? data retention?)
- ⚠️ **Reliability** (API uptime, rate limit 429 errors)
- ⚠️ **Brave Search replacement?** (2000 req/month, 60s spacing, web-only)
  - Complementary: Grok for real-time + X, Brave for stable web
  - Replacement: If Grok covers all use cases at lower cost

### For The Constituent (TAR)

**Potential benefits**:
- ✅ **Phase 3 Twitter skill accelerator** (native X search vs custom implementation)
- ✅ **Real-time constitutional discourse monitoring** (X mentions, debates)
- ✅ **Community engagement tracking** (search mentions, track discussions)
- ✅ **DeepSearch for research** (complex queries, constitutional research)

**Trade-offs**:
- ⚠️ **L2 approval required** (external API integration, public content risk)
- ⚠️ **Cost management** (rate limits, budget allocation)
- ⚠️ **Privacy** (constitutional discussions logged by xAI?)

---

## 🔐 Security Considerations

**Checklist** (from docs/security-checklist-skills.md):

1. ✅ **Read SKILL.md completely**: ⏳ Pending (skill not yet accessible)
2. ⏳ **Check source reputation**: Christopher Stanley credibility unknown
3. ⏳ **Audit code**: Not available for review yet
4. ✅ **Verify sandbox boundaries**: External API = network scope (documented)
5. ⏳ **Test in isolated session**: After skill availability confirmed
6. ⏳ **Document in security log**: Pending evaluation completion

**Risk assessment** (preliminary):

- **External dependency**: 🟡 MEDIUM RISK
  - xAI API = third-party service (uptime, pricing changes, data retention)
  - API key security critical (never hardcode, environment variable only)
  
- **Data privacy**: 🟡 MEDIUM RISK
  - Queries sent to xAI servers (log retention unknown)
  - Constitutional discussions may be sensitive (TAR use case)
  - Need xAI data retention policy review

- **Cost control**: 🟢 LOW RISK
  - Token-based pricing = predictable costs
  - $175/month free credits = generous buffer
  - Rate limits prevent runaway spending

- **Supply chain**: 🟡 MEDIUM RISK
  - Christopher Stanley source unverified
  - Skill code not audited
  - ClawHub distribution mechanism unclear

---

## 📊 Comparison: Grok vs Brave Search

| Feature | xAI Grok API | Brave Search API (Current) |
|---------|--------------|----------------------------|
| **Real-time** | ✅ Yes (live web + X) | ⚠️ Delayed (indexed) |
| **X (Twitter)** | ✅ Native integration | ❌ No |
| **Cost** | $0.20/M tokens + $175/mo free | Free (2000 req/month) |
| **Rate limits** | Per-tier (RPM, TPM) | 1 req/s, 2000/month |
| **Caching** | ✅ Yes (prefix matching) | ❌ No |
| **Citations** | ✅ Yes (domain filters) | ✅ Yes (titles, URLs, snippets) |
| **Images** | ✅ Understanding (256-1792 tokens) | ❌ No |
| **Privacy** | ⚠️ xAI servers | ✅ Brave (privacy-focused) |
| **Reliability** | ⏳ Unknown (new integration) | ✅ Proven (active use) |

**Recommendation**: **COMPLEMENTARY** usage
- **Brave**: Stable web searches, privacy-focused, free tier sufficient
- **Grok**: Real-time web + X monitoring, image understanding, complex queries

---

## 🚦 Next Actions

### Immediate (Priority: 🟡 HIGH)

1. **Verify Christopher Stanley post**:
   - Direct X verification (after Brave rate limit reset)
   - Check @cstanley timeline for Grok skill announcement
   - Confirm skill repository (GitHub link?)

2. **Locate skill source**:
   - GitHub search: "grok skill openclaw"
   - ClawHub alternative commands (if exists)
   - Community Discord/X mentions

3. **xAI API key acquisition research**:
   - Sign-up process: https://console.x.ai
   - Team tier options (free, paid)
   - API key generation steps

4. **Cost estimation** (revised with server-side tools):
   - Ralph veille usage: ~50 queries/day (web + X monitoring)
   - **Token consumption**: 1K prompt + 2K completion per query
     - Monthly: 50 * 30 * 3K = 4.5M tokens/month
     - Cost: (4.5M * $0.20) / 1M = $0.90/month
   - **Server-side tool calls**: web_search + x_search
     - Monthly: 50 calls/day * 30 days = 1,500 calls/month
     - Cost: (1,500 / 1,000) * $5 = $7.50/month
   - **Total monthly cost**: $0.90 + $7.50 = **$8.40/month**
   - **Budget status**: Well within $175/month free credits ✅

### If Skill Confirmed (Priority: 🟡 MEDIUM)

5. **Security audit**:
   - Review skill code (GitHub repository)
   - Check API key handling (environment variable? encrypted?)
   - Verify sandbox boundaries (network calls documented?)
   - Test in isolated Ralph workspace

6. **Pilot test** (L1 autonomous, Ralph workspace only):
   - Install grok skill (isolated test)
   - Test `search_web()` vs Brave Search (quality, speed, cost)
   - Test `search_x()` for Twitter monitoring (@TheConstituent_, crypto × AI)
   - Measure token consumption (actual vs estimate)
   - Document findings

7. **Integration decision** (L2 Blaise approval):
   - Report pilot test results
   - Propose Ralph veille enhancement (Brave + Grok complementary)
   - Propose The Constituent Phase 3 Twitter skill (if applicable)
   - Budget allocation ($0.90/month estimated, $175 free credits available)

### If Skill NOT Confirmed (Priority: 🟢 LOW)

8. **Document as speculative**:
   - Update research file (signal unverified)
   - Monitor for future announcements (Christopher Stanley, ClawHub, OpenClaw releases)
   - Revisit when confirmed

---

## 📝 Summary

**Status**: 🟡 **INVESTIGATION IN PROGRESS**

**Findings**:
- xAI Grok API: ✅ CONFIRMED (real-time web + X, $0.20/M tokens, $175/mo free)
- Grok skill for OpenClaw: ❓ NOT CONFIRMED (ClawHub not found, skill not installed)
- Christopher Stanley post: ⏳ NOT VERIFIED (Brave rate limited, need direct check)

**Potential value**:
- Ralph veille: 🟡 HIGH (real-time web + X monitoring, cost-effective)
- The Constituent: 🟡 MEDIUM (Phase 3 Twitter skill accelerator, research tool)

**Risks**:
- External dependency: 🟡 MEDIUM (xAI API, uptime, pricing changes)
- Privacy: 🟡 MEDIUM (queries logged by xAI, constitutional discussions sensitive)
- Supply chain: 🟡 MEDIUM (skill source unverified, code not audited)

**Recommendation**:
1. Verify signal (Christopher Stanley post, skill repository)
2. If confirmed: Security audit → Pilot test → Integration decision (L2)
3. If not confirmed: Monitor for future availability, document xAI API research

**Timeline**: After OpenClaw 2.15 update complete ✅ (update finished 08:35 UTC)

---

**Next update**: After Brave rate limit reset (verify Christopher Stanley post, GitHub search)
