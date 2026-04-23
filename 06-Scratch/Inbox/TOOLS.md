# TOOLS.md - Environment-Specific Notes

_Last updated: 2026-04-05_

## Preferred Accounts

- **Email:** david@geniinow.com (sending from Homegenii.com)
- **GitHub:** davidmschy
- **ERPNext:** localhost:8081 (Administrator/admin)

## Tracker / Sheets

- ERPNext at localhost:8081 for all business data
- gws (Google Workspace CLI) - see below for setup issues

## Local Environment

- **Mac Studio:** M3 Max, 128GB RAM
- **Ollama:** http://localhost:11434 (models: qwen2.5:7b, gemma3:12b, nomic-embed-text, nemotron-3-nano:30b, qwen2.5:1.5b)
- **OpenCode:** /Applications/OpenCode.app
- **CodexBar:** /Applications/CodexBar.app

## Target Market

- Factory-built housing development
- Land acquisition & development
- Mobile factories near developments
- Subcontracted labor model
- Construction/bridge loans, LP investors
- Exit: refinance or sell at Certificate of Occupancy
- Target tenants: assisted living, section 8-style groups at market rate

## Tactical Operating Rules

1. Don't start messages with filler ("Great question", "I'd be happy to help")
2. One sentence if it fits
3. Don't ask permission to do things you can just do
4. Update memory/ in real-time
5. Check clawchief/tasks.md before proposing tasks
6. Only escalate blockers, not routine questions
7. Ask clarifying questions before starting any project

---

## Google Workspace (gws) Setup - WORKING

### Installation
```bash
npm install -g @googleworkspace/cli
```

### Authentication (Working Method)
The gws CLI has OAuth callback issues. Workaround:

1. **gcloud auth is working** - run `gcloud auth login` and complete browser
2. **gws uses keyring backend** - requires OAuth flow that fails on callback

### Current Status
- gcloud: ✅ Authenticated as david@geniinow.com (limited scopes)
- gws: ❌ OAuth callback never completes despite browser approval
- APIs enabled: 46 (Drive, Gmail, Calendar, Docs, Sheets, etc.)
- Project: genii-464014
- OAuth Client: [REDACTED]

### What Works Directly (via gcloud token)
```bash
# User info
gcloud auth print-access-token | xargs -I {} curl -s -H "Authorization: Bearer {}" "https://www.googleapis.com/oauth2/v2/userinfo"

# Calendar (read-only)
TOKEN=$(gcloud auth print-access-token)
curl -s -H "Authorization: Bearer $TOKEN" "https://www.googleapis.com/calendar/v3/calendars/primary"
```

### gws Auth Commands
```bash
# Setup (requires browser)
gws auth setup --project genii-464014

# Login (requires browser - callback issue)
gws auth login -s drive,gmail,calendar

# Status check
gws auth status

# Drive files
gws drive files list --params '{"pageSize": 5}'

# Gmail messages
gws gmail users messages list --params '{"maxResults": 5}'

# Calendar events
gws calendar events list --params '{"maxResults": 5}'
```

### Env Vars for gws
```bash
export GOOGLE_WORKSPACE_CLI_CLIENT_ID="[REDACTED]"
export GOOGLE_WORKSPACE_CLI_CLIENT_SECRET="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"
```

---

## Ollama Models

| Model | Size | Purpose |
|-------|------|---------|
| qwen2.5:7b | 4.7GB | Tool calling (agents) |
| gemma3:12b | 8.1GB | Reasoning |
| nemotron-3-nano:30b | 24GB | Heavy lifting |
| qwen2.5:1.5b | 986MB | Quick queries |
| nomic-embed-text:latest | 274MB | Embeddings |

**Note:** gemma4 not available on Ollama yet - need newer Ollama version.

### Commands
```bash
ollama list
ollama pull <model>
ollama run <model>
```

---

## ERPNext

- **URL:** http://localhost:8081
- **API:** /api/resource/<DocType>
- **Membrane:** Authenticated (davidmschy@gmail.com)

### API Examples
```bash
# Login
curl -s -X POST http://localhost:8081/api/method/login -d "usr=Administrator&pwd=admin" -c cookies.txt

# Get data
curl -s -b cookies.txt "http://localhost:8081/api/resource/Lead" | jq '.'
```

---

## GitHub

- **Account:** davidmschy
- **Token:** PAT with full scopes

### Repos
- homegenii-{amber,robert,tony,john,mark,david,aryan,shared}
- genii-*, fbx-*, GolfGenii, erpnext-cli, polymarket-bot

---

## Known Quirks

- gws OAuth needs browser completion on Mac (callback fails silently)
- Use gcloud for direct API calls as workaround
- OpenClaw runs via LaunchAgent (pid varies)
- Memory: always write to memory/YYYY-MM-DD.md, never rely on memory

---

## Company Structure (from David)

**Genii** - Parent brand for AI tools/SaaS (real estate focus)
**FBX** - Merged into Homegenii.com (was marketplace for modular housing)
**Homegenii** - Current housing business (factory-built, land dev, construction loans)
**GolfGenii** - Dad's golf content (book, course, training aids)

Active: Homegenii, Genii (tech), GolfGenii
Dormant: FBX (merged)