# Constitution Skill — Development Plan Phase 2A

**Date:** 2026-02-15  
**Agent:** The Constituent  
**Purpose:** Track 27 constitutional articles status, search, validate compliance  
**Estimated effort:** 1 day (design + implement + test + deploy)

---

## Skill Specification

### Metadata
- **Name:** `constitution`
- **Version:** 1.0.0
- **Author:** Ralph (LobsterOps)
- **Target agent:** The Constituent
- **Security level:** 🟢 Low risk (read-only local files, no network)

### Functions

#### 1. `constitution_status()`
**Purpose:** Scan all 27 articles, return status overview

**Logic:**
```javascript
// Read constitution-status.json
// Return: {
//   total: 27,
//   published: 7,  // Articles with title
//   draft: 20,     // Articles without title
//   articles: [
//     {id: 1, title: "...", status: "published", progress: 100},
//     {id: 2, title: null, status: "draft", progress: 0},
//     ...
//   ]
// }
```

**Data source:** `workspace-constituent/data/constitution-status.json` (to create)

**Output:** JSON structured

---

#### 2. `constitution_search(query)`
**Purpose:** Find articles by keyword

**Parameters:**
- `query` (string) — Keyword or phrase

**Logic:**
```javascript
// Load all published articles from constitution/ directory
// Search title + body for query (case-insensitive)
// Return matching articles with excerpt
```

**Output:** 
```json
{
  "matches": [
    {
      "articleId": 1,
      "title": "Founding Principles",
      "excerpt": "...matching text...",
      "relevance": 0.95
    }
  ]
}
```

---

#### 3. `constitution_validate(proposal)`
**Purpose:** Check if proposal complies with constitutional articles

**Parameters:**
- `proposal` (object) — Proposal text, type, scope

**Logic:**
```javascript
// Parse proposal
// Compare against relevant constitutional articles
// Identify conflicts or compliance gaps
// Return validation report
```

**Output:**
```json
{
  "compliant": true,
  "conflicts": [],
  "recommendations": ["Article 5 may require citizen vote"]
}
```

---

#### 4. `constitution_version_control()`
**Purpose:** Track amendments history

**Logic:**
```javascript
// Read amendments log (amendments.json)
// Return version history, ratification dates, changes
```

**Output:**
```json
{
  "currentVersion": "1.0",
  "amendments": [
    {
      "id": 1,
      "ratificationDate": "2026-02-10",
      "changes": ["Article 15 added"],
      "status": "ratified"
    }
  ]
}
```

---

## File Structure

```
~/.openclaw/workspace/skills-dev/constitution/
├── index.js              # Main skill logic
├── SKILL.md              # Documentation
├── package.json          # Dependencies (if needed)
├── test.js               # Unit tests
└── data/
    ├── constitution-status.json    # Article status tracker
    └── amendments.json             # Amendment history
```

---

## Data Files Schema

### constitution-status.json
```json
{
  "lastUpdated": "2026-02-15T08:00:00Z",
  "articles": [
    {
      "id": 1,
      "title": "Founding Principles",
      "status": "published",
      "progress": 100,
      "publishedDate": "2026-02-10"
    },
    {
      "id": 2,
      "title": null,
      "status": "draft",
      "progress": 0,
      "publishedDate": null
    }
    // ... 25 more articles
  ]
}
```

### amendments.json
```json
{
  "version": "1.0",
  "amendments": [
    {
      "id": 1,
      "proposedDate": "2026-02-05",
      "ratificationDate": "2026-02-10",
      "changes": ["Article 15: Conflict Resolution added"],
      "status": "ratified",
      "voteResults": {
        "for": 85,
        "against": 10,
        "abstain": 5
      }
    }
  ]
}
```

---

## Security Checklist Application

### Pre-Development Review

#### 1. Source Verification ✅
- **Author:** Ralph (trusted, internal development)
- **Repository:** LobsterOps workspace (no external dependencies)
- **Community feedback:** N/A (custom skill)
- **Red flags:** None

#### 2. Capability Audit ✅
- **File access scope:** `workspace-constituent/data/` only (read-only)
- **Network calls:** None
- **API keys:** None required
- **System commands:** None
- **Permissions:** Read-only file access

**Risk assessment:** 🟢 **Low** (local file operations, no network, no mutations)

#### 3. Code Review ✅
- **Dependencies:** Zero external npm packages (pure Node.js fs/path)
- **No eval/exec:** Confirmed
- **Input validation:** Required (sanitize query, proposal inputs)
- **Error handling:** Proper try/catch blocks
- **Logging:** No sensitive data logged

#### 4. Sandboxing Validation ✅
- **File boundaries:** Cannot escape `workspace-constituent/data/`
- **Network restrictions:** N/A (no network calls)
- **Resource limits:** Expected <2s execution, minimal memory

---

## Development Workflow

### Step 1 — Design SKILL.md ✅
Location: `workspace/skills-dev/constitution/SKILL.md`

Document:
- Skill name, version, author
- Functions (4 detailed above)
- Data sources
- Security constraints
- Usage examples

**ETA:** 30 minutes

---

### Step 2 — Implement index.js
Location: `workspace/skills-dev/constitution/index.js`

Structure:
```javascript
const fs = require('fs');
const path = require('path');

const DATA_DIR = path.join(process.env.WORKSPACE_ROOT, 'data');

function constitution_status() {
  // Implementation
}

function constitution_search(query) {
  // Implementation
}

function constitution_validate(proposal) {
  // Implementation
}

function constitution_version_control() {
  // Implementation
}

module.exports = {
  constitution_status,
  constitution_search,
  constitution_validate,
  constitution_version_control
};
```

**ETA:** 2-3 hours

---

### Step 3 — Write Tests (test.js)
Location: `workspace/skills-dev/constitution/test.js`

Tests:
```javascript
// Test 1: constitution_status returns 27 articles
// Test 2: constitution_search finds article by keyword
// Test 3: constitution_validate detects conflicts
// Test 4: constitution_version_control returns amendments
```

Run tests:
```bash
node workspace/skills-dev/constitution/test.js
```

**Success criteria:**
- ✅ All 4 functions execute without errors
- ✅ Output format matches spec
- ✅ File access limited (no unauthorized reads)
- ✅ Performance <2s per function

**ETA:** 1 hour

---

### Step 4 — Test via Ralph (Isolated Session)
**Why Ralph first?** Safe isolated environment before Constituent deployment.

Test protocol:
1. Load skill in Ralph workspace
2. Send Telegram message: `test constitution_status`
3. Observe response, check logs
4. Test all 4 functions
5. Verify no errors, acceptable performance

**Success criteria:**
- ✅ Ralph can call all 4 functions
- ✅ Responses structured correctly
- ✅ No crashes, no timeout
- ✅ Logs clean (no errors)

**ETA:** 30 minutes

---

### Step 5 — Security Checklist Final Audit
Before deploying to Constituent, re-apply checklist:

- ✅ Source code reviewed (no malicious code)
- ✅ File access validated (workspace-constituent/data/ only)
- ✅ No network calls confirmed
- ✅ Input sanitization verified
- ✅ Test results acceptable

Document audit:
```bash
echo "Constitution Skill Security Audit - $(date)" >> memory/constituent-skills-security.md
echo "Status: ✅ PASSED" >> memory/constituent-skills-security.md
```

**ETA:** 15 minutes

---

### Step 6 — Deploy to Constituent
**Final deployment:**

```bash
# Copy skill to Constituent workspace
cp -r ~/.openclaw/workspace/skills-dev/constitution \
  ~/.openclaw/workspace-constituent/skills/

# Create data directory
mkdir -p ~/.openclaw/workspace-constituent/data/

# Initialize data files (constitution-status.json, amendments.json)
# Copy from template or create empty

# Reload Constituent gateway
openclaw gateway reload  # SIGUSR1, zero downtime

# Test via Constituent Telegram
# Send message to Constituent bot: "test constitution_status"
```

**Success criteria:**
- ✅ Constituent lists `constitution` skill in available tools
- ✅ Constituent can execute all 4 functions
- ✅ No errors in Constituent logs
- ✅ Response quality acceptable

**ETA:** 30 minutes

---

### Step 7 — Documentation & Monitoring

**Update files:**
1. `MEMORY.md` — "Constitution skill deployed (Phase 2A)"
2. `memory/2026-02-15.md` — Deployment log
3. `memory/constituent-skills-security.md` — Audit complete
4. `HEARTBEAT.md` — Add weekly skill usage check

**Monitoring (weekly):**
- Check skill usage logs (Constituent)
- Verify no errors/crashes
- Measure performance (response times)
- Update data files (constitution-status.json when articles published)

**ETA:** 15 minutes

---

## Total Estimated Time

| Phase | Task | ETA |
|-------|------|-----|
| Step 1 | Design SKILL.md | 30 min |
| Step 2 | Implement index.js | 2-3 hours |
| Step 3 | Write tests | 1 hour |
| Step 4 | Test via Ralph | 30 min |
| Step 5 | Security audit | 15 min |
| Step 6 | Deploy Constituent | 30 min |
| Step 7 | Document & monitor | 15 min |
| **TOTAL** | **~5-6 hours** | **1 working day** |

---

## Success Criteria Summary

**Phase 2A Constitution Skill = COMPLETE when:**
1. ✅ All 4 functions operational
2. ✅ Security checklist passed
3. ✅ Deployed to Constituent workspace
4. ✅ Tested via Constituent Telegram (no errors)
5. ✅ Documentation updated (MEMORY.md, logs)
6. ✅ Monitoring configured (HEARTBEAT weekly check)

---

## Next Skills (Phase 2A Continuation)

After Constitution skill deployed:
1. **Citizen skill** (2 days) — Citizen registry, approval workflow
2. **Governance skill** (2 days) — Proposals, voting, activation

**Phase 2A Complete:** 3 skills operational (constitution + citizen + governance)

---

**Status:** Planning complete, awaiting OpenClaw upgrade validation before development start.
