# Homegenii Infrastructure Setup — Status Report
*Generated: April 18, 2026*

## COMPLETED

### 1. Team Agents Deployed (9 total)
All agents running via PM2 with persistent memory and live GWS/ERPNext integration:

| Agent | Role | Email | GWS | ERPNext | Status |
|-------|------|-------|-----|---------|--------|
| agent-david | CEO/Lead | david@homegenii.com | ✅ | ✅ | online |
| agent-amber | Business Manager | amber@homegenii.com | ✅ | ✅ | online |
| agent-robert | Draftsman & Facility Manager | robert@homegenii.com | ✅ | ✅ | online |
| agent-tony | Labor & Project Coordinator | tony@homegenii.com | ✅ | ✅ | online |
| agent-john | Land Acquisitions | john@homegenii.com | ✅ | ✅ | online |
| agent-mark | Realtor & Sales | mark@homegenii.com | ✅ | ✅ | online |
| agent-antonio | QC Manager | antonio@homegenii.com | ✅ | ✅ | online |
| agent-syed | Software Engineer | — | ⏸️ | ⏸️ | online (stub) |
| agent-aryan | Accountant & CFO | — | ⏸️ | ⏸️ | online (stub) |

**Syed & Aryan:** Contractors without Workspace accounts yet. Agents are running but not connected to GWS/ERPNext. Will be activated when accounts are provisioned.

**Location:** `~/.openclaw/agents/{agent}/`
**Runtime:** PM2 (auto-starts on boot)
**Runtime Script:** `~/.openclaw/runtime/agent_runner.py`

### 2. Google Workspace Integration (DWD)
**Method:** Service Account with Domain-Wide Delegation
**GSA:** `genii-agents@genii-464014.iam.gserviceaccount.com`
**Domain:** `homegenii.com`

**Verified Access:**
- ✅ Gmail (read/unread, message listing)
- ✅ Calendar (event listing)
- ✅ Drive (file listing, upload, share)
- ⏸️ Chat (API not enabled — non-critical)

### 3. ERPNext Multi-Company Setup
**URL:** http://localhost:8081
**Login:** Administrator / admin

Companies created:
| Company | Abbr | Type | Status |
|---------|------|------|--------|
| Homegenii | HG | Operating | ✅ |
| FBX Holdings | FBX | Group | ✅ |
| Genii Technologies | GEN | Operating | ✅ |
| GolfGenii | GG | Operating | ✅ |

**API Keys Generated:** All active employees have API keys for programmatic access.

### 4. Infrastructure Services
| Service | Status | URL/Location |
|---------|--------|--------------|
| ERPNext | ✅ Running | http://localhost:8081 |
| Docker | ✅ Running | 6 containers |
| PM2 | ✅ Running | 9 agent processes |
| Ollama | ✅ Running | http://localhost:11434 |
| OpenClaw | ✅ Running | localhost:18789 |

### 5. Shared Resources
- **Google Drive Client:** `~/.openclaw/agents/shared/google_drive.py` (with user impersonation)
- **Agent Runtime:** `~/.openclaw/runtime/agent_runner.py` (Gmail + Calendar + ERPNext polling)

## IN PROGRESS / NEXT STEPS

### Immediate (Today)
1. **Agent SOUL.md updates** — Add detailed role directives for each agent
2. **ERPNext email migration** — Consider migrating ERPNext users from @geniinow.com to @homegenii.com for consistency
3. **Obsidian vault setup** — Create shared vault for agent memory and team docs
4. **GitHub integration** — Configure agent access to repos for code/deployment tasks

### This Weekend
1. **Agent capability expansion:**
   - Drive file operations per agent
   - Gmail send/reply capabilities
   - ERPNext task creation and assignment
   - Cross-agent messaging via shared memory
2. **FBX platform setup:**
   - Install dependencies (`npm install`)
   - Set up PostgreSQL database
   - Configure ERPNext webhooks
3. **Financial stack research:**
   - Coinbase CDP
   - Lithic/Stripe Issuing
   - Alpaca API

### Monday
1. Team onboarding session
2. ERPNext training for agents
3. Lead flow testing (homegenii.com → ERPNext)

## KEY LOCATIONS

```
~/.openclaw/
├── agents/              # Team agent configs & SOUL files
│   ├── amber/
│   ├── robert/
│   ├── tony/
│   ├── john/
│   ├── mark/
│   ├── antonio/
│   ├── syed/
│   ├── aryan/
│   └── david/
├── runtime/             # PM2 agent runner
│   ├── agent_runner.py
│   └── ecosystem.config.js
├── workspace/           # Documentation & memory
│   ├── TOOLS.md
│   ├── AGENTS.md
│   └── MEMORY.md
└── skills/              # Agent capabilities

~/fbx-platform/          # FBX marketplace platform
├── .env                 # ERPNext API credentials
├── client/              # React frontend
├── server/              # Express backend
└── db/                  # Drizzle ORM

ERPNext: http://localhost:8081
```

## KNOWN ISSUES
1. **Chat scope** — Google Chat API not enabled; not blocking core functionality
2. **ERPNext user emails** — Still using @geniinow.com; functional but inconsistent with GWS domain
3. **Gmail snippets** — Message snippets in agent logs are empty; need individual message fetch for content
4. **Contractor agents** — Syed & Aryan agents are stubs pending workspace account creation

---

*Last updated: April 18, 2026 3:35 PM PDT*
