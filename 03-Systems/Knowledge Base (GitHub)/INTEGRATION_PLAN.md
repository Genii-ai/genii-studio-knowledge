# Genii Studio — Master Integration Plan
**Status:** Assessment Complete | **Date:** 2026-04-19

---

## Current State: What's ALREADY Working

| System | Status | Details |
|--------|--------|---------|
| **ERPNext** | ✅ Connected | API key active. Cloudflare 403 bypassed via User-Agent header. 0 projects (fresh instance). |
| **Mattermost** | ✅ Connected | system-bot token active. Posts to #engineering and #town-square. 10 human users + 10 bot accounts created. |
| **Google Workspace** | ✅ Connected | gws CLI authenticated (david@geniinow.com). Drive, Calendar, Gmail, Sheets, Docs all accessible. |
| **GitHub** | ✅ Connected | davidmschy authenticated with full admin scopes. 10+ repos exist (geniinow-projects, homegenii-*, genii-ops, genii-landing-pages). |
| **LM Studio** | ✅ Running | Gemma 4 e4b, qwen3-coder-30b, nomic-embed-text all loaded on localhost:1234. Configured as Hermes custom_provider. |
| **Ollama** | ✅ Running | 3 models (nomic-embed-text, qwen2.5:1.5b, nemotron-3-nano:30b). |
| **Cloudflare Tunnels** | ✅ Running | cad.geniinow.com, erp.geniinow.com, mm.geniinow.com all live. |
| **FreeCAD/Blender** | ✅ Installed | Full pipeline tested end-to-end (Parkview APN 449-341-009). |
| **PM2 Agents** | ✅ Running | All 10 employee agents (david, amber, robert, tony, john, mark, aryan, antonio, syed) + shared + cloudflared. |
| **OpenCode** | ✅ Installed | /opt/homebrew/bin/opencode |
| **Claude Code** | ✅ Installed | /opt/homebrew/bin/claude |
| **Codex** | ✅ Installed | /opt/homebrew/bin/codex |
| **Kimi** | ✅ Installed | /opt/homebrew/bin/kimi |

**Bottom line:** The infrastructure is 85% built. What's left is wiring, credential distribution, and the per-employee agent architecture.

---

## Phase 1: Complete Core Connections (1-2 hours)

### 1.1 Google Workspace — Fine-tune
- **Done:** Auth works. Drive files list, Calendar list, Gmail all tested.
- **Todo:**
  - [ ] Create shared Drive folder `Genii Studio / Projects` with structured subfolders
  - [ ] Verify each agent can access GWS via service account or shared credentials
  - [ ] Set up GDrive upload from pipeline (SitePlan → GDrive → link in ERPNext)

### 1.2 GitHub — Repo Structure & Webhooks
- **Done:** Auth works. Repos exist.
- **Todo:**
  - [ ] Create `genii-studio-pipeline` repo for CI/CD of plan generation
  - [ ] Configure branch protection on `geniinow-projects`
  - [ ] Add GitHub Action: auto-deploy landing pages from `genii-landing-pages`
  - [ ] Set up webhook: GitHub push → Mattermost #engineering notification

### 1.3 LM Studio — Hermes Provider Fix
- **Done:** Server running, models loaded, custom_provider entry exists.
- **Problem:** Hermes config has `models: []` empty list.
- **Todo:**
  - [ ] Add model entries to `~/.hermes/config.yaml` for gemma-4-e4b
  - [ ] Test inference through Hermes (not just direct curl)

### 1.4 Obsidian — Create Genii Studio Vault
- **Done:** Obsidian installed, existing "Enjoy Real Estate" vault found.
- **Todo:**
  - [ ] Create `~/GeniiStudio/Obsidian` vault
  - [ ] Symlink or copy key markdown files (SYSTEM.md, AGENT_PLAN.md, COLLABORATION_WORKFLOW.md)
  - [ ] Set up templates for project notes, compliance reports, meeting notes

---

## Phase 2: Per-Employee AI Agent Architecture (2-3 hours)

### 2.1 Agent Identity & Credentials
Each of the 10 employees gets an agent with:
- **Mattermost:** Bot account already created (e.g., `david-ai-agent`).
- **ERPNext:** API key/secret (need to generate per-user or use shared service account).
- **GitHub:** Personal access token or SSH key for their repo.
- **GWS:** Access via gws CLI (shared credentials acceptable since all are internal).
- **Google Calendar:** Read access for meeting prep.
- **Gmail:** Read access for triage and task extraction.

### 2.2 Agent Role Configuration
| Agent | Role | Primary Systems | Key Responsibilities |
|-------|------|-----------------|---------------------|
| **david-ai-agent** | CEO/Dealmaker | All | Pipeline oversight, deal flow, cross-team coordination |
| **amber-ai-agent** | Business Manager | ERPNext, GWS, Mattermost | Project tracking, scheduling, ops reports |
| **robert-ai-agent** | Draftsman | FreeCAD, GitHub, ERPNext | CAD generation, plan revisions, HCD compliance |
| **tony-ai-agent** | Labor Coord | ERPNext, Mattermost | Subcontractor scheduling, QC coordination |
| **john-ai-agent** | Land Broker | ERPNext, GWS, Mattermost | APN lookup, zoning analysis, loan tracking |
| **mark-ai-agent** | Realtor/Modular Sales | ERPNext, GWS, Mattermost | Lead management, sales pipeline, land acquisition |
| **aryan-ai-agent** | Accountant/CFO | ERPNext, Sheets, Gmail | Financial reports, LP investor tracking, construction loans |
| **antonio-ai-agent** | QC Manager | ERPNext, FreeCAD, Mattermost | HCD inspections, facility coordination, compliance checks |
| **syed-ai-agent** | Software Engineer | GitHub, ERPNext, Mattermost | System maintenance, API integrations, bug fixes |
| **shared-ai-agent** | General Assistant | All | Catch-all for team-wide tasks, knowledge retrieval |

### 2.3 Agent Deployment Plan
- **Current:** All agents run via `agent_wrapper.py` in PM2.
- **Missing:** Each agent loads the same generic config. Need role-specific configs.
- **Todo:**
  - [ ] Create `agents/configs/david.json`, `amber.json`, etc. with role-specific prompts, system instructions, and allowed tools.
  - [ ] Update `agent_wrapper.py` to load role config from `AGENT_ROLE` env var.
  - [ ] Restart all PM2 agents with updated configs.
  - [ ] Test: Each agent responds with role-specific behavior in Mattermost DM.

### 2.4 Agent → GWS Integration
- **Todo:**
  - [ ] Each agent can read/write Google Calendar (meeting prep, scheduling)
  - [ ] Each agent can read Gmail (triage, extract action items)
  - [ ] Each agent can read/write Google Sheets (reports, tracking)
  - [ ] Each agent can read/write Google Docs (meeting notes, SOPs)

### 2.5 Agent → ERPNext Integration
- **Todo:**
  - [ ] Create service account or shared API key for agent access
  - [ ] Each agent can query projects, tasks, documents
  - [ ] Each agent can create/update tasks in their domain
  - [ ] Auto-link CAD files and compliance reports to ERPNext projects

---

## Phase 3: End-to-End Automation & Workflows (3-4 hours)

### 3.1 Pipeline → GDrive Upload
- **Flow:** Address input → SitePlan generated → Upload FCStd + STP + data.json to GDrive → Link in ERPNext
- **Todo:**
  - [ ] Add `gws drive files +upload` step to `FullPipeline.py`
  - [ ] Create folder structure: `Genii Studio / Projects / {APN} /`
  - [ ] Return shareable link and store in ERPNext project `custom_cad_drive_link`

### 3.2 Pipeline → GitHub Publish
- **Flow:** Plan generated → Commit to `geniinow-projects` under `projects/{APN}/`
- **Todo:**
  - [ ] Add `git add / commit / push` step to `FullPipeline.py`
  - [ ] Auto-generate README.md per project with metadata
  - [ ] Tag commits with project name + compliance score

### 3.3 Pipeline → Mattermost Notification
- **Flow:** Plan complete → Post to #engineering with links to ERPNext, GDrive, GitHub
- **Todo:**
  - [ ] Enhance `MattermostBot.py` to include GDrive and GitHub links
  - [ ] Add compliance pass/fail summary inline
  - [ ] Tag relevant human users (@david, @robert) based on project stage

### 3.4 Pipeline → ERPNext Project Auto-Creation
- **Flow:** Address input → Auto-create ERPNext Project + Tasks + Document links
- **Todo:**
  - [ ] Fix `ERPNext_Integration.py` to use proper User-Agent header
  - [ ] Test with Parkview data
  - [ ] Auto-create task list: Civil → Structural → Title 24 → HCD → Permit

### 3.5 Daily Standup Automation
- **Flow:** 8am daily → Agent queries ERPNext + Calendar + Mattermost → Generates standup report → Posts to #general
- **Todo:**
  - [ ] Create `macros/DailyStandup.py`
  - [ ] Schedule via cron or PM2 cron
  - [ ] Include: yesterday's completed tasks, today's meetings, blocked items

---

## Phase 4: Advanced Agent Tools (4-6 hours)

### 4.1 Claude Code / Codex / OpenCode / Kimi Integration
- **Todo:**
  - [ ] Create `launch-*` scripts for each agent role (already have launch-claude.sh, launch-kimi.sh)
  - [ ] Add Codex launch script
  - [ ] Test: Each CLI agent can read project context and execute tasks
  - [ ] Document when to use which agent (Claude for architecture, Codex for coding, Kimi for Chinese docs, OpenCode for general)

### 4.2 LM Studio Deep Integration
- **Todo:**
  - [ ] Use Gemma 4 e4b for compliance analysis (large context window)
  - [ ] Use qwen3-coder for code generation and review
  - [ ] Use nomic-embed for RAG over project documents
  - [ ] Configure fallback: if LM Studio down → use Ollama → use cloud API

### 4.3 GBrain Knowledge System
- **Todo:**
  - [ ] Ensure GBrain is indexing all project documents
  - [ ] Each agent queries GBrain before external API calls (Brain First)
  - [ ] Add compliance rules, SOPs, and HCD regulations to knowledge base

---

## Execution Order (Recommended)

1. **Fix LM Studio Hermes config** (5 min) — unblock AI provider
2. **Create Obsidian vault** (15 min) — give David easy markdown access
3. **Test ERPNext project creation** (15 min) — validate the core data flow
4. **Create per-agent role configs** (30 min) — differentiate the 10 agents
5. **Add GDrive upload to pipeline** (30 min) — complete the artifact flow
6. **Add GitHub publish to pipeline** (20 min) — version control for plans
7. **Wire full end-to-end test** (20 min) — Parkview → all systems
8. **Deploy daily standup** (30 min) — first automated workflow
9. **Document everything** (30 min) — SOPs for each agent

**Total estimated time:** ~3 hours of focused work.

---

## Open Questions for David

1. **ERPNext per-user API keys:** Should each employee get their own API key, or use a shared service account?
2. **GitHub org vs personal:** Should we create a `geniinow` GitHub org to consolidate repos?
3. **Obsidian sync:** Do you want the Obsidian vault in iCloud (current) or Git-backed for team access?
4. **KimiCode CLE:** Do you want me to install and configure this now, or is the current stack sufficient?
5. **Landing pages:** Should I set up the CI/CD pipeline for `genii-landing-pages` now, or after the core integrations?
