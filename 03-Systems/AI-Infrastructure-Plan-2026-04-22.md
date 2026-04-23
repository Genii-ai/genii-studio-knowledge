# AI Infrastructure Plan — April 22, 2026

**Status:** Ready for review  
**Scope:** Full agent skill provisioning, Google Workspace integration, GitHub workflow, local model access, Obsidian canonical interface  
**Estimated Execution Time:** 2–3 hours  
**Risk Level:** Low (all additive, no destructive ops)

---

## Current State Snapshot

| System | Status | Notes |
|--------|--------|-------|
| GitHub (davidmschy) | ✅ Auth'd | Admin on Genii-ai org. 3 repos. Full scopes. |
| Google Workspace CLI (gws/gog) | ⚠️ Installed, **not auth'd** | gws 0.22.5, gog v0.12.0. Need OAuth flow. |
| LM Studio (Gemma 4 E4B) | ✅ Running | localhost:1234. Model loaded and responding. |
| OpenClaw Gateway | ✅ Running | Port 18789, public via gateway.geniinow.com |
| Mattermost | ✅ Running | 10 agents deployed via PM2, paired to employees |
| Cloudflare Tunnel | ✅ Running | 6 hostnames mapped |
| ERPNext Docker | ✅ Running | v16.10.1, multi-company, custom doctypes live |
| Obsidian Vault | ✅ Exists | PARA structure in place at `~/obsidian/genii-studio/` |
| Codex | ✅ Installed | Ready for coding delegation |
| Claude Code | ✅ Installed | Ready for architecture/review tasks |
| Opencode | ✅ Installed | Ready for fast iteration |
| Kimi Code | ✅ Installed | Ready for specific tasks |
| AntiGravity | ❌ Not installed | Needs install if we want it |
| PM2 | ✅ Running | 10 mm-agents + cloudflared |

**Agent Model Routing (Current):**
- 7 agents → MiniMax M2.5 (default)
- Aryan, Syed → Gemma 4 E4B (local/free)
- No skills assigned yet to any agent

---

## Phase 1: Google Workspace Integration (30–45 min)

**Goal:** Every agent can read/write Gmail, Calendar, Drive, Sheets, and Docs on behalf of employees.

**Prerequisite Decision:** Service Account vs OAuth
- **Service Account** = background automation, no user consent needed, domain-wide delegation required
- **OAuth** = acts as the user, requires consent, better for "send email as me"

**Recommendation:** Hybrid approach
1. **OAuth** for Rufio (me) and David — personal access to Drive/Gmail/Calendar
2. **Service Account** for automated agents (Amber, Aryan, etc.) — for reports, syncing, scheduled tasks

**Execution Steps:**
1. `gws auth login` → OAuth flow for primary account (david@geniinow.com)
2. `gog auth login` → OAuth flow for Drive/Sheets/Docs heavy lifting
3. Verify access: list Drive files, read Calendar, triage Gmail
4. Create service account key in GCP for background agents
5. Share critical Drive folders with service account email
6. **Test end-to-end:** Agent reads a Sheet → updates ERPNext → sends Gmail summary

**Open Question:** Do you have a GCP project set up with Workspace APIs enabled, or do we need to create one?

---

## Phase 2: GitHub Workflow & Project Structure (30 min)

**Goal:** All landing pages, deployments, and projects live in version control with CI/CD.

**Current Repos:**
- `Genii-ai/genii-crm` — ERPNext CRM module
- `Genii-ai/geniinow-projects` — Pipeline/automation
- `Genii-ai/genii-studio-knowledge` — Knowledge base & SOPs

**Needed Structure:**
```
Genii-ai/
  genii-crm/              ✅ exists
  genii-erpnext/          📁 custom doctypes, scripts, patches
  genii-landing-pages/    📁 Next.js/Vercel landing pages
  genii-cad/              📁 CAD/BIM interface + FreeCAD scripts
  genii-manufacturing/    📁 CNC paths, SIP panel specs
  genii-ops/              📁 Docker compose, infra configs
  genii-studio-knowledge/ ✅ exists
```

**Execution Steps:**
1. Create the 4 new repos with README + MIT license
2. Set branch protection rules (main requires PR)
3. Add `vercel-deployment` skill configs for landing pages
4. Create a `genii-ops` repo with Docker Compose files, Cloudflare config, and PM2 ecosystem files
5. Move `~/.cloudflared/config.yml`, `~/.openclaw/openclaw.json` (sanitized), and PM2 configs into `genii-ops`
6. **Symlink strategy:** Live configs stay in place, but canonical versions live in GitHub

---

## Phase 3: LM Studio + Local Model Routing (15 min)

**Goal:** Confirm Gemma 4 E4B is usable by OpenClaw agents, benchmark it, and document when to route local.

**Current:**
- LM Studio running at localhost:1234
- Model: `google/gemma-4-e4b`
- OpenClaw already has provider alias `gemma-4`

**Execution Steps:**
1. Test API: `curl http://localhost:1234/v1/chat/completions` with a coding task
2. Verify Aryan and Syed agents are actually routing to LM Studio (check logs)
3. Benchmark: Compare latency/quality of Gemma 4 vs MiniMax M2.5 on a typical ERPNext query
4. Document the decision matrix in this vault:
   - MiniMax M2.5 → General chat, creative tasks, external knowledge
   - Gemma 4 E4B → Code review, financial analysis (Aryan), software dev (Syed), privacy-sensitive ops
   - Gemini 3.1 Flash → Explicit pay-per-use when asked

---

## Phase 4: Obsidian as Canonical Interface (20 min)

**Goal:** You interact with all my markdown output here. No more ad-hoc files.

**Current Vault Structure:**
```
00-Command Center/
01-Projects/
02-Agents/
03-Systems/
04-Team-Memory/
05-Runbooks/
06-Scratch/
99-Archive/
99-Daily-Notes/
```

**Execution Steps:**
1. Add this vault path to my workspace config so I read/write here by default
2. Create templates:
   - `Templates/Decision-Log.md` — for architectural decisions
   - `Templates/Runbook.md` — for step-by-step procedures
   - `Templates/Project-Plan.md` — for new project specs
   - `Templates/Agent-Config.md` — for per-employee agent setup
3. Move all ad-hoc markdown files from `~/geniinow-projects/` into `01-Projects/` or `99-Archive/`
4. Establish convention: I write plans here first, then execute

---

## Phase 5: Per-Employee Agent Skill Provisioning (45 min)

**Goal:** Each of the 10 agents has the right skills and tools for their human's role.

**Current:** 10 agents running via PM2, all with `skills: []`

**Proposed Skill Mapping:**

| Employee | Role | Primary Model | Skills to Enable | ERPNext Access | GWS Access |
|----------|------|---------------|------------------|----------------|------------|
| David | CEO/Deals | MiniMax M2.5 | business-development, executive-assistant, github-cli, gws-workflow | Full | Full (OAuth) |
| Amber | Business Mgr | MiniMax M2.5 | executive-assistant, gws-workflow, erpnext, daily-task-manager | Full | Full (OAuth) |
| Robert | Draftsman/Facility | MiniMax M2.5 | erpnext, gws-docs, gws-drive | Project/Asset | Drive/Docs |
| Tony | Labor/QC | MiniMax M2.5 | erpnext, gws-tasks | Project/QC | Tasks/Drive |
| John | Land Acq/Broker | MiniMax M2.5 | business-development, gws-drive, gws-gmail | CRM/Leads | Drive/Gmail |
| Mark | Realtor/Modular Sales | MiniMax M2.5 | business-development, gws-gmail, gws-calendar | CRM/Opportunities | Gmail/Calendar |
| Aryan | Accountant/CFO | Gemma 4 E4B | erpnext, gws-sheets, gws-docs | Full Financial | Sheets/Docs |
| Antonio | QC/HCD | MiniMax M2.5 | erpnext, gws-tasks, gws-drive | QC/Compliance | Drive/Tasks |
| Syed | Software Eng | Gemma 4 E4B | github-cli, github-workflow, codex-cli, opencode-controller, lmstudio-subagents | Dev access | GitHub only |

**Execution Steps:**
1. Update `~/.openclaw/openclaw.json` to add `skills` array per agent
2. Restart PM2 agents: `pm2 restart all`
3. Test each agent via Mattermost DM: "What skills do you have?"
4. Verify ERPNext MCP tools are discoverable by each agent
5. Verify Google Workspace tools are discoverable after Phase 1

---

## Phase 6: Coding Agent Arsenal Configuration (20 min)

**Goal:** Know which coding agent to deploy for what task.

**Current Tools:** Codex, Claude Code, Opencode, Kimi Code

**Decision Matrix:**

| Tool | Best For | Cost | Speed | When to Use |
|------|----------|------|-------|-------------|
| **Claude Code** | Architecture, complex refactoring, review | Paid | Medium | Starting new modules, design decisions |
| **Codex** | Implementation, tests, CRUD | Paid | Fast | "Build this endpoint", "Write these tests" |
| **Opencode** | Fast iteration, exploration, small fixes | Free/Open | Fast | Quick experiments, learning codebases |
| **Kimi Code** | Long-context analysis, Chinese docs | Paid | Medium | Large file analysis, documentation |
| **AntiGravity** | (Not installed) UI-heavy, visual coding | ? | ? | Install if you want a GUI alternative |

**Execution Steps:**
1. Verify each tool can access Genii-ai repos
2. Set default model/config per tool:
   - Claude Code → Claude 4 Sonnet (default)
   - Codex → GPT-5-codex
   - Opencode → whatever's configured
3. Test: "Clone genii-crm, review the hooks, suggest improvements"
4. **Optional:** Install AntiGravity if you want a GUI coding environment

---

## Execution Order & Dependencies

```
Phase 1 (GWS Auth) ──► Phase 5 (Agent Skills)
     │                      │
     ▼                      ▼
Phase 4 (Obsidian) ◄── Phase 2 (GitHub)
     │
     ▼
Phase 3 (LM Studio) ──► Phase 6 (Coding Agents)
```

**Critical Path:** Phase 1 → Phase 5. Everything else can parallelize.

---

## Success Criteria

- [ ] `gws auth status` shows authenticated user
- [ ] Agent David can list Drive files and send Gmail via Mattermost DM
- [ ] 4 new GitHub repos created with READMEs
- [ ] `genii-ops` repo contains canonical configs
- [ ] Aryan agent routes to Gemma 4 and answers a financial question correctly
- [ ] This Obsidian vault has templates and is my default output location
- [ ] Each agent reports its skills correctly in Mattermost
- [ ] Claude Code and Codex can both clone and review genii-crm

---

## Blockers / Open Questions

1. **GCP Project:** Do you have a Google Cloud project with Workspace APIs enabled? If not, we need to create one.
2. **AntiGravity:** Do you want it installed? It's another coding UI.
3. **Service Account:** Should I create a service account for automated agents, or stick to OAuth per-user?
4. **Kimi Code CLE:** You mentioned getting me CLE — is that a license file or account?
5. **Vercel:** Do you have a Vercel account/team for landing page deployments?

---

**Next Step:** Review this plan. Once you approve or edit, I'll execute Phase 1 first.
