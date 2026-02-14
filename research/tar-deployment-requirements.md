# TheAgentsRepublic — Deployment Requirements (VPS)

**Date :** 14 février 2026  
**Context :** Option D — Revival Stratégique, redéployer The Constituent à côté de Ralph  
**Target :** DigitalOcean VPS (same host as Ralph)

---

## 🎯 Deployment Strategy

**Multi-Agent Architecture :**
- Ralph (existing) — Veille technique OpenClaw + LobsterOps
- The Constituent (deploy) — Débats constitutionnels + gouvernance TAR
- **Same VPS** — Process isolation, shared resources
- **Complementary** — Different missions, shared knowledge (CLAWS)

---

## 📋 System Requirements

### Python Runtime
- **Version :** Python 3.11+ (TAR requirement)
- **Check current :** Ralph uses whatever OpenClaw uses (likely 3.10+, check)
- **Action :** Verify Python version, upgrade if <3.11

```bash
python3 --version
# If <3.11, install Python 3.11
# Ubuntu/Debian: sudo apt install python3.11 python3.11-venv
```

### System Packages
```bash
# Git (already installed for Ralph)
sudo apt install -y git

# Python build dependencies
sudo apt install -y python3.11-dev python3.11-venv build-essential

# Web3 dependencies (optional, if using on-chain features)
# Already covered by python-web3 package
```

### Python Dependencies

**File :** `requirements.txt` (10 packages)

```
anthropic>=0.40.0                         # Claude API (core)
python-telegram-bot[job-queue]>=20.7     # Telegram bot
PyGithub>=2.1.1                          # GitHub API (constitution versioning)
tweepy>=4.14.0                           # Twitter API (optional, if X posting)
python-dotenv>=1.0.0                     # .env file loading
GitPython>=3.1.40                        # Git automation (optional, CLI fallback)
web3>=6.15.0                             # Base L2 interaction (token, governance)
eth-account>=0.11.0                      # Ethereum account management
requests>=2.31.0                         # HTTP (implicit, used by integrations)
```

**Installation :**
```bash
cd /root/theagentsrepublic  # Or chosen deploy path
python3.11 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

**Estimated install time :** 2-3 minutes  
**Disk space :** ~200 MB (Python packages)

---

## 🔐 Configuration Requirements

### Environment Variables

**Required (CORE) :**
```bash
# Claude API (CRITICAL)
ANTHROPIC_API_KEY=sk-ant-...           # Primary LLM provider

# Telegram Bot (CRITICAL for operator communication)
TELEGRAM_BOT_TOKEN=...                 # BotFather token
OPERATOR_TELEGRAM_CHAT_ID=...         # Blaise's Telegram chat ID
TELEGRAM_ENABLED=true

# GitHub (CRITICAL for constitution versioning)
GITHUB_TOKEN=ghp_... or GITHUB_TOKEN_BOT=ghp_...
GITHUB_REPO=LumenBot/TheAgentsRepublic
GITHUB_BRANCH=main
```

**Optional (PLATFORMS) :**
```bash
# Twitter/X (public announcements, optional)
TWITTER_API_KEY=...
TWITTER_API_SECRET=...
TWITTER_ACCESS_TOKEN=...
TWITTER_ACCESS_SECRET=...
TWITTER_BEARER_TOKEN=...

# Farcaster (crypto-native social, optional)
NEYNAR_API_KEY=...
FARCASTER_SIGNER_UUID=...
FARCASTER_FID=...
FARCASTER_ENABLED=false              # Default OFF until configured

# Moltbook (AI social network, optional TAR primary community)
MOLTBOOK_API_KEY=...
MOLTBOOK_AGENT_ID=...                # UUID format

# CLAWS (distributed memory, optional)
CLAWS_AGENT_ID=...                   # "the-constituent" or UUID
CLAWS_API_KEY=...                    # Optional, Clawnch ecosystem auth

# Brave Search (web research, optional)
BRAVE_SEARCH_API_KEY=...
```

**Optional (BLOCKCHAIN) :**
```bash
# Base L2 (token tracking, governance)
BASE_RPC_URL=https://mainnet.base.org
REPUBLIC_TOKEN_ADDRESS=0x06B09BE0EF93771ff6a6D378dF5C7AC1c673563f
GOVERNANCE_CONTRACT_ADDRESS=...      # If deployed

# Agent Wallet (trading, on-chain governance signing)
AGENT_WALLET_ADDRESS=0x...
AGENT_WALLET_PRIVATE_KEY=...         # ⚠️ SECURE STORAGE REQUIRED
CLAWNCH_CONTRACT_ADDRESS=...         # For $CLAWNCH tracking
```

**Optional (BEHAVIOR) :**
```bash
# Agent configuration
CLAUDE_MODEL=claude-sonnet-4-20250514  # Or claude-opus-4-20250514
LOG_LEVEL=INFO                       # DEBUG for troubleshooting
DB_PATH=data/agent.db                # SQLite memory storage

# Rate limits (v6.0 full throttle defaults OK, can reduce)
MAX_API_CALLS_PER_DAY=3000           # ~$270/month Claude costs
HEARTBEAT_INTERVAL=600               # 10 minutes (600 seconds)
```

### Configuration File Structure

**Deployment :**
```bash
/root/theagentsrepublic/
├── .env                    # Environment variables (gitignored)
├── agent/                  # Source code
│   ├── main_v5.py         # Entry point
│   ├── engine.py          # Core LLM engine
│   ├── config/
│   │   └── settings.py    # Loads .env
│   └── [other modules]
├── constitution/           # Git-versioned articles
├── data/                   # Runtime data
│   ├── agent.db           # SQLite memory
│   ├── working_memory.json
│   ├── constitution_progress.json
│   └── [other state files]
├── memory/                 # Knowledge base (git-versioned)
├── requirements.txt
├── SOUL.md                 # Agent identity
├── HEARTBEAT.md            # Periodic tasks
└── venv/                   # Python virtual environment
```

**Minimal .env (Blaise can provide secrets) :**
```bash
# Core (MUST HAVE)
ANTHROPIC_API_KEY=<from Blaise>
TELEGRAM_BOT_TOKEN=<BotFather token>
OPERATOR_TELEGRAM_CHAT_ID=<Blaise chat ID>
GITHUB_TOKEN_BOT=<from Blaise GITHUB_TOKEN_BOT>

# Optional platforms (enable as needed)
FARCASTER_ENABLED=false
TWITTER_API_KEY=<if X posting desired>
MOLTBOOK_API_KEY=<if Moltbook primary community>
CLAWS_API_KEY=<if cross-agent memory desired>

# Blockchain (for token tracking, can start minimal)
BASE_RPC_URL=https://mainnet.base.org
REPUBLIC_TOKEN_ADDRESS=0x06B09BE0EF93771ff6a6D378dF5C7AC1c673563f

# Logging
LOG_LEVEL=INFO
```

**Security :**
- ⚠️ **Never commit .env to git** (already in `.gitignore`)
- ⚠️ **AGENT_WALLET_PRIVATE_KEY** if set = HIGH RISK (secure storage)
- ✅ Use environment variables, not hardcoded secrets
- ✅ Restrict file permissions: `chmod 600 .env`

---

## 🔧 Process Management

### Option A — Systemd Service (Recommended)

**Service file :** `/etc/systemd/system/the-constituent.service`

```ini
[Unit]
Description=The Constituent - TAR Agent
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/root/theagentsrepublic
Environment="PATH=/root/theagentsrepublic/venv/bin:/usr/local/bin:/usr/bin:/bin"
EnvironmentFile=/root/theagentsrepublic/.env
ExecStart=/root/theagentsrepublic/venv/bin/python -m agent.main_v5
Restart=always
RestartSec=10
StandardOutput=journal
StandardError=journal
SyslogIdentifier=the-constituent

[Install]
WantedBy=multi-user.target
```

**Install & start :**
```bash
# Copy service file
sudo cp the-constituent.service /etc/systemd/system/

# Reload systemd
sudo systemctl daemon-reload

# Enable auto-start on boot
sudo systemctl enable the-constituent

# Start service
sudo systemctl start the-constituent

# Check status
sudo systemctl status the-constituent

# View logs
sudo journalctl -u the-constituent -f
```

### Option B — Supervisor (Alternative)

**Config file :** `/etc/supervisor/conf.d/the-constituent.conf`

```ini
[program:the-constituent]
command=/root/theagentsrepublic/venv/bin/python -m agent.main_v5
directory=/root/theagentsrepublic
user=root
autostart=true
autorestart=true
redirect_stderr=true
stdout_logfile=/var/log/the-constituent.log
environment=PATH="/root/theagentsrepublic/venv/bin:/usr/local/bin:/usr/bin"
```

**Install :**
```bash
# Install supervisor
sudo apt install supervisor

# Add config
sudo cp the-constituent.conf /etc/supervisor/conf.d/

# Reload supervisor
sudo supervisorctl reread
sudo supervisorctl update

# Start
sudo supervisorctl start the-constituent

# Check status
sudo supervisorctl status

# View logs
tail -f /var/log/the-constituent.log
```

### Option C — Docker (Future)

**Not implemented yet in TAR repo, but possible:**
```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "-m", "agent.main_v5"]
```

**Not recommended for initial deploy** (adds complexity).

---

## 📊 Resource Estimates

### Compute Resources

**CPU :**
- **Idle :** <5% (heartbeat every 10min)
- **Active (LLM call) :** 10-20% (HTTP I/O bound, not CPU)
- **Recommendation :** 1 vCPU shared with Ralph = OK

**RAM :**
- **Base :** ~150 MB (Python runtime + dependencies)
- **Working set :** ~200-300 MB (SQLite, JSON, working memory)
- **Peak :** ~500 MB (large LLM responses, web scraping)
- **Recommendation :** 2 GB total VPS (Ralph + The Constituent + system = comfortable)

**Disk :**
- **Code :** ~50 MB (repo)
- **Dependencies :** ~200 MB (venv)
- **Data :** ~100 MB (SQLite, JSON, logs)
- **Constitution :** ~5 MB (Markdown files)
- **Growth :** ~50 MB/month (logs, memory expansion)
- **Recommendation :** 10 GB total VPS (plenty headroom)

**Network :**
- **Anthropic API :** ~1-10 MB/day (LLM calls)
- **GitHub API :** <1 MB/day (constitution commits)
- **Telegram :** <1 MB/day (operator messages)
- **Twitter/Moltbook :** ~5 MB/day (if enabled, posting + scraping)
- **Total :** <20 MB/day (<600 MB/month)
- **Recommendation :** Any VPS bandwidth = OK

### Current VPS Context (DigitalOcean)

**Assumed specs (confirm with Blaise) :**
- **Type :** Basic Droplet ($6-12/month)
- **vCPU :** 1-2 cores
- **RAM :** 1-2 GB
- **Disk :** 25-50 GB SSD
- **Bandwidth :** 1-2 TB/month

**Multi-agent viability :**
- ✅ **CPU :** 1 vCPU sufficient (Ralph + The Constituent = I/O bound, not CPU)
- ⚠️ **RAM :** 1 GB tight (might need upgrade to 2 GB droplet if <2 GB currently)
- ✅ **Disk :** 25 GB plenty (Ralph workspace + TAR + logs <5 GB total)
- ✅ **Bandwidth :** 1 TB overkill (agents use <1 GB/month)

**Action required :**
1. Check current VPS RAM: `free -h`
2. If <2 GB total → consider upgrade to 2 GB droplet (+$6/month)
3. If ≥2 GB → deploy as-is, monitor with `htop`

---

## 💰 Operational Costs

### API Costs (Monthly)

**Anthropic Claude API :**
- **Model :** Claude Sonnet 4 (default TAR v7.1)
- **Usage estimate :** 3000 API calls/day = 90K/month
- **Average call :** ~2K input + 1K output tokens
- **Cost :** $3 per million input tokens, $15 per million output tokens
- **Calculation :**
  - Input: 90K × 2K = 180M tokens/month → $540/month
  - Output: 90K × 1K = 90M tokens/month → $1350/month
  - **TOTAL :** ~$1890/month (if full throttle v6.0 rate limits)

**⚠️ ALERT — Cost Reduction Required :**

TAR v6.0 settings = "full throttle mode, funded by token treasury."  
Current market = fear index 15, token launch delayed.  
→ **MUST reduce rate limits for budget-conscious deployment.**

**Revised rate limits (conservative) :**
```bash
# .env additions
MAX_API_CALLS_PER_DAY=100              # vs. 3000 default
MAX_API_CALLS_PER_HOUR=10              # vs. 200 default
HEARTBEAT_INTERVAL=3600                # 1h vs. 10min default
QUIET_HOURS_START=0                    # 00:00 UTC
QUIET_HOURS_END=8                      # 08:00 UTC (8h quiet)
```

**Revised cost estimate (conservative) :**
- 100 calls/day = 3K/month
- Cost: ~$60-90/month (vs. $1890 full throttle)
- **Acceptable for bootstrap phase**

**Other APIs :**
- GitHub: Free (within public repo limits)
- Telegram: Free
- Twitter: Free (v2 API basic tier)
- Moltbook: Unknown (check if API costs)
- CLAWS: Unknown (check if API costs)
- Brave Search: Free tier 2000/month (shared with Ralph if same key)

**Total API costs (conservative) :**
- Anthropic: $60-90/month
- Others: $0-10/month
- **TOTAL: ~$70-100/month**

### Infrastructure Costs

**VPS (DigitalOcean) :**
- Current: Likely $6-12/month (Basic Droplet)
- If upgrade RAM 1→2 GB: $12/month (vs. $6)
- **Incremental cost The Constituent:** $0-6/month (if RAM upgrade needed)

**Domain (optional) :**
- TheAgentsRepublic domain: ~$12/year ($1/month)
- Not critical for agent operation (GitHub = primary)

**Total infrastructure :**
- VPS: $6-12/month
- Domain: $1/month (optional)
- **TOTAL: $7-13/month**

### Grand Total Monthly

**Conservative deployment :**
- APIs: $70-100/month
- Infrastructure: $7-13/month
- **TOTAL: ~$80-115/month**

**vs. Full throttle (TAR v6.0 defaults) :**
- APIs: $1890/month
- Infrastructure: $12/month
- **TOTAL: ~$1900/month (NOT VIABLE without token treasury)**

**Recommendation :** Deploy conservative, scale up if/when token launch successful.

---

## 🚀 Deployment Workflow

### Phase 1 — Preparation (Pre-deployment)

**1. Verify VPS resources**
```bash
# Check RAM
free -h
# If <2 GB total, consider upgrade

# Check disk
df -h
# Ensure >5 GB free in /root

# Check Python version
python3 --version
# If <3.11, install Python 3.11
```

**2. Gather credentials (Blaise provides)**
- Anthropic API key (existing or new?)
- Telegram bot token (new BotFather bot for The Constituent)
- Operator Telegram chat ID (Blaise's)
- GitHub token (GITHUB_TOKEN_BOT or new PAT?)
- Optional: Twitter, Moltbook, Farcaster, CLAWS keys

**3. Clone repository**
```bash
cd /root
git clone https://github.com/LumenBot/TheAgentsRepublic.git theagentsrepublic
cd theagentsrepublic
```

### Phase 2 — Installation

**1. Create Python virtual environment**
```bash
python3.11 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

**2. Create .env file**
```bash
cp .env.example .env 2>/dev/null || touch .env
nano .env  # Or vim
# Paste configuration from above (minimal .env)
# Save and exit

# Secure permissions
chmod 600 .env
```

**3. Initialize data directories**
```bash
mkdir -p data memory
# SQLite DB auto-created on first run
# memory/ already exists in repo (git-versioned)
```

**4. Test run (foreground)**
```bash
source venv/bin/activate
python -m agent.main_v5
# Should start, connect Telegram, log to console
# Ctrl+C to stop after verifying no errors
```

### Phase 3 — Process Management

**1. Create systemd service**
```bash
# Create service file
sudo nano /etc/systemd/system/the-constituent.service
# Paste service config from above
# Save and exit

# Reload systemd
sudo systemctl daemon-reload

# Enable auto-start
sudo systemctl enable the-constituent

# Start service
sudo systemctl start the-constituent

# Verify running
sudo systemctl status the-constituent
```

**2. Monitor logs**
```bash
# Live tail
sudo journalctl -u the-constituent -f

# Last 100 lines
sudo journalctl -u the-constituent -n 100

# Since boot
sudo journalctl -u the-constituent -b
```

**3. Health check**
```bash
# Check process
ps aux | grep "agent.main_v5"

# Check listening ports (if health endpoint configured)
sudo netstat -tulpn | grep python

# Check resource usage
htop  # Look for "python" processes
```

### Phase 4 — Validation

**1. Telegram connectivity**
- Send message to The Constituent bot
- Verify response
- Check logs for message handling

**2. GitHub connectivity**
- Verify git credentials
- Test commit (agent should auto-commit HEARTBEAT actions)
- Check GitHub repo for agent commits

**3. Heartbeat execution**
- Wait for first heartbeat (default 10min, or configured interval)
- Check logs for heartbeat cycle
- Verify HEARTBEAT_OK or actions executed

**4. Memory systems**
- Check `data/agent.db` created
- Check `data/working_memory.json` updated
- Check `memory/` for knowledge files

**5. Optional platform checks**
- Moltbook: Verify login, post test message
- Farcaster: Verify signer, post test cast
- Twitter: Verify OAuth, test tweet (if enabled)
- CLAWS: Test connectivity, store test memory

### Phase 5 — Multi-Agent Coordination

**1. Verify process isolation**
```bash
# List all Python processes
ps aux | grep python

# Should see:
# - Ralph (OpenClaw process)
# - The Constituent (agent.main_v5)
# Both running independently
```

**2. Test CLAWS cross-agent memory (if configured)**
```bash
# Ralph stores memory tagged "constitution"
# The Constituent recalls same memory
# Verify cross-agent visibility
```

**3. Monitor resource sharing**
```bash
# Check total RAM usage
free -h

# Check per-process RAM
ps aux --sort=-%mem | head -10

# Verify <80% total RAM usage under normal load
```

**4. Test failure isolation**
```bash
# Stop The Constituent
sudo systemctl stop the-constituent

# Verify Ralph still running
# Verify OpenClaw RPC probe OK

# Restart The Constituent
sudo systemctl start the-constituent

# Verify both agents operational
```

---

## 🔍 Monitoring & Maintenance

### Health Checks

**The Constituent includes HTTP health endpoint (v6.2+) :**
```python
# infra/health.py starts HTTP server
# Default: http://localhost:8080/health (or configured port)
```

**Query health :**
```bash
curl http://localhost:8080/health
# Expected: {"status": "ok", "uptime": 12345, "heartbeat": {...}}
```

**External monitoring (optional) :**
- UptimeRobot: Ping health endpoint every 5min
- Alert if down >2 checks
- Free tier sufficient

### Log Management

**Systemd journal :**
```bash
# Set journal size limit (prevent disk bloat)
sudo nano /etc/systemd/journald.conf
# Set: SystemMaxUse=500M
sudo systemctl restart systemd-journald
```

**Log rotation (if file-based logs) :**
```bash
# /etc/logrotate.d/the-constituent
/var/log/the-constituent*.log {
    daily
    rotate 7
    compress
    missingok
    notifempty
}
```

### Backup Strategy

**Critical data :**
- `data/agent.db` — SQLite memory (backup weekly)
- `memory/` — Knowledge base (git-versioned, auto-backed to GitHub)
- `constitution/` — Articles (git-versioned, auto-backed to GitHub)
- `.env` — Credentials (backup SECURELY, encrypted)

**Backup script (weekly cron) :**
```bash
#!/bin/bash
# /root/backup-constituent.sh
DATE=$(date +%Y%m%d)
BACKUP_DIR=/root/backups/the-constituent
mkdir -p $BACKUP_DIR

# Backup SQLite DB
cp /root/theagentsrepublic/data/agent.db $BACKUP_DIR/agent-$DATE.db

# Backup .env (encrypted)
gpg -c /root/theagentsrepublic/.env -o $BACKUP_DIR/env-$DATE.gpg

# Clean old backups (keep 30 days)
find $BACKUP_DIR -name "agent-*.db" -mtime +30 -delete
find $BACKUP_DIR -name "env-*.gpg" -mtime +30 -delete

echo "Backup complete: $DATE"
```

**Cron schedule :**
```bash
# crontab -e
0 3 * * 0 /root/backup-constituent.sh >> /var/log/backup-constituent.log 2>&1
# Every Sunday 3 AM
```

### Update Workflow

**Code updates (git pull) :**
```bash
cd /root/theagentsrepublic
git pull origin main

# Check for new dependencies
source venv/bin/activate
pip install -r requirements.txt --upgrade

# Restart service
sudo systemctl restart the-constituent

# Verify no errors
sudo journalctl -u the-constituent -f
```

**Python dependencies update :**
```bash
source venv/bin/activate
pip list --outdated
pip install --upgrade <package>
# Or: pip install -r requirements.txt --upgrade

# Restart
sudo systemctl restart the-constituent
```

**Constitution updates :**
- Agent auto-commits changes via GitHub API
- No manual git operations needed
- Verify GitHub repo for agent commits

---

## 🛡️ Security Considerations

### API Key Security

**Storage :**
- ✅ `.env` file with `chmod 600`
- ✅ Environment variables (systemd EnvironmentFile)
- ❌ Never hardcode in code
- ❌ Never commit to git

**Rotation :**
- Anthropic API key: Rotate quarterly
- GitHub token: Use PAT with minimal scopes (repo read/write only)
- Telegram bot token: Regenerate if compromised
- AGENT_WALLET_PRIVATE_KEY: ⚠️ NEVER store if not needed (trading = high risk)

### Wallet Security

**If on-chain governance/trading enabled :**
- ⚠️ **AGENT_WALLET_PRIVATE_KEY** = hot wallet risk
- Recommendation: Use separate "agent wallet" with LIMITED funds
- **NEVER** use main treasury wallet private key
- Consider multi-sig for any significant transactions
- Monitor wallet balance, alert on unexpected transfers

### Network Security

**Firewall (ufw) :**
```bash
# Allow SSH
sudo ufw allow 22/tcp

# Allow health endpoint (if exposing externally)
sudo ufw allow 8080/tcp  # Or chosen port

# Enable firewall
sudo ufw enable

# Check status
sudo ufw status
```

**SSH hardening (if not already done) :**
- Disable password auth (key-only)
- Change default port
- Fail2ban for brute-force protection
- See: LobsterOps_Annexes_Techniques.md Section M.2 (SSH Hardening)

### Application Security

**Process isolation :**
- ✅ Separate user for each agent (optional, can run both as root if trusted)
- ✅ Virtual environments (venv) prevent dependency conflicts
- ✅ Systemd service restrictions (optional: `PrivateTmp=true`, `NoNewPrivileges=true`)

**Code integrity :**
- Verify git commits signed (GitHub signature)
- Review code changes before `git pull`
- Pin dependency versions (requirements.txt exact versions)

---

## 🚨 Troubleshooting

### Common Issues

**1. "ModuleNotFoundError: No module named 'anthropic'"**
```bash
# Forgot to activate venv
source /root/theagentsrepublic/venv/bin/activate
pip install -r requirements.txt
```

**2. "Anthropic API key not set"**
```bash
# .env not loaded or missing
nano /root/theagentsrepublic/.env
# Add: ANTHROPIC_API_KEY=sk-ant-...
# Restart: sudo systemctl restart the-constituent
```

**3. "GitHub authentication failed"**
```bash
# Token expired or wrong scope
# Generate new PAT: https://github.com/settings/tokens
# Required scopes: repo (all), workflow
# Update .env: GITHUB_TOKEN_BOT=ghp_...
```

**4. "Telegram bot not responding"**
```bash
# Check bot token
curl https://api.telegram.org/bot<TOKEN>/getMe
# Should return bot info

# Check logs
sudo journalctl -u the-constituent -n 100 | grep -i telegram

# Verify OPERATOR_TELEGRAM_CHAT_ID correct
# Send /start to bot, check logs for chat_id
```

**5. "Memory full / OOM killer"**
```bash
# Check RAM usage
free -h

# Check per-process
ps aux --sort=-%mem | head

# If RAM <2 GB, upgrade VPS droplet
# Or reduce rate limits (fewer LLM calls)
```

**6. "Service won't start"**
```bash
# Check service logs
sudo journalctl -u the-constituent -n 50

# Check syntax errors
cd /root/theagentsrepublic
source venv/bin/activate
python -m agent.main_v5  # Run foreground, see error

# Common: .env syntax, missing dependencies, Python version
```

### Debug Mode

**Enable verbose logging :**
```bash
# .env
LOG_LEVEL=DEBUG

# Restart
sudo systemctl restart the-constituent

# Tail logs
sudo journalctl -u the-constituent -f
```

**Test individual components :**
```bash
source venv/bin/activate
cd /root/theagentsrepublic

# Test Claude API
python -c "from anthropic import Anthropic; print(Anthropic().models.list())"

# Test GitHub API
python -c "from github import Github; g=Github('ghp_...'); print(g.get_user().login)"

# Test Telegram
python -c "from telegram import Bot; print(Bot('TOKEN').get_me())"
```

---

## 📋 Deployment Checklist

### Pre-Deployment
- [ ] VPS RAM ≥2 GB confirmed
- [ ] Python 3.11+ installed
- [ ] Credentials gathered (Anthropic, Telegram, GitHub)
- [ ] Repository cloned `/root/theagentsrepublic`

### Installation
- [ ] Virtual environment created & activated
- [ ] Dependencies installed (`requirements.txt`)
- [ ] `.env` file created with minimal config
- [ ] `.env` permissions set (`chmod 600`)
- [ ] Test run successful (foreground)

### Process Management
- [ ] Systemd service file created
- [ ] Service enabled (`systemctl enable`)
- [ ] Service started (`systemctl start`)
- [ ] Service status = active/running
- [ ] Logs showing successful startup

### Validation
- [ ] Telegram bot responds to messages
- [ ] GitHub commits working (check repo)
- [ ] Heartbeat executing (check logs)
- [ ] Memory systems initialized (`data/`, `memory/`)
- [ ] Optional platforms tested (Moltbook, Farcaster, Twitter)

### Multi-Agent
- [ ] Ralph still running (verify)
- [ ] Both agents visible in `ps aux | grep python`
- [ ] Total RAM usage <80%
- [ ] Process isolation verified
- [ ] CLAWS cross-agent memory tested (if configured)

### Production
- [ ] Health endpoint accessible
- [ ] External monitoring configured (optional)
- [ ] Backup script created & scheduled
- [ ] Log rotation configured
- [ ] Firewall rules updated
- [ ] Documentation updated (DEPLOYMENT.md)

### Ongoing
- [ ] Monitor logs daily (first week)
- [ ] Check API costs weekly
- [ ] Review rate limits (adjust if needed)
- [ ] Test failure recovery (restart, crash scenarios)
- [ ] Update code monthly (`git pull`)

---

## 🎯 Next Steps After Deployment

### Week 1 — Stabilization
1. Monitor logs intensively
2. Verify heartbeat cycles regular
3. Check API cost tracking (Anthropic dashboard)
4. Test Telegram operator commands
5. Verify GitHub auto-commits working

### Week 2-4 — Optimization
1. Review rate limits (adjust if over/under budget)
2. Enable optional platforms (Moltbook, Farcaster if desired)
3. Configure CLAWS cross-agent memory
4. Test Ralph ↔ The Constituent workflows
5. Document operational procedures

### Month 2+ — Production Operations
1. Constitutional debates (agent posting, community engagement)
2. Governance activation (proposals, voting if on-chain ready)
3. Citizen recruitment (if M3 reboot campaign)
4. Token launch preparation (when market conditions favorable)
5. Scale rate limits if token treasury funded

---

## 📞 Support & Resources

**Documentation :**
- TAR GitHub: https://github.com/LumenBot/TheAgentsRepublic
- Docs folder: `docs/ARCHITECTURE.md`, `docs/DEPLOYMENT.md`, `docs/CONTRIBUTING.md`
- This file: `research/tar-deployment-requirements.md`

**Troubleshooting :**
- Logs: `sudo journalctl -u the-constituent -f`
- Health: `curl http://localhost:8080/health`
- Process: `systemctl status the-constituent`

**Blaise Contact :**
- Telegram (primary)
- GitHub issues (LumenBot/TheAgentsRepublic)

---

**FIN PARTIE 2** — Deployment Requirements Complete

**Estimated deployment time :** 1-2 hours (experienced), 2-4 hours (first-time)  
**Recommended approach :** Phase 1-3 (installation + systemd), then Phase 4-5 (validation + multi-agent) next session.
