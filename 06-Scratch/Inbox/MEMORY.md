# MEMORY.md - Long-Term Memory

_Last updated: 2026-04-06_

## Who I Am

- **Name:** Rufio
- **Role:** AI Lead Admin for David Schy
- **Emoji:** ⚓

## About David

- **Name:** David Schy
- **Timezone:** America/Los_Angeles (PDT)
- **Business:** Factory-built housing developer
  - Land acquisition & development
  - Prefab/modular manufacturing (mobile factories near developments)
  - Subcontracted labor
  - Construction loans, bridge loans, LP investors
  - Exit: refinance or sell at Certificate of Occupancy
  - Target tenants: assisted living, section 8-style groups at market rate
- **Goal:** Zero human developer — fully AI-driven operations

## Team

| Name | Role | ERPNext | GitHub |
|------|------|---------|--------|
| Amber | Business Manager | ✅ | ✅ homegenii-amber |
| Robert | Manufacturing Manager | ✅ | ✅ homegenii-robert |
| Tony | QC + Labor | ✅ | ✅ homegenii-tony |
| John | Land Acquisition | ✅ | ✅ homegenii-john |
| Mark | Sales + Acquisitions | ✅ | ✅ homegenii-mark |
| Aryan | CFO | ⏸️ pending | ✅ homegenii-aryan |

## Infrastructure

- **OpenClaw:** Running (Google Gemini API configured: gemini-2.0-flash)
- **ERPNext:** localhost:8081 (via Membrane CLI, authenticated)
- **GitHub:** davidmschy authenticated
- **Ollama:** Models loaded (qwen2.5:7b, gemma3:12b, nomic-embed-text, nemotron-3-nano:30b)
- **Google Workspace:** Paused (gws/gog auth issues - deprioritized)
- **ClawFlows:** 113 workflows installed, auto-scheduler running

## Active Projects

1. Set up team agent instances (Amber, Robert, Tony, John, Mark, Aryan)
2. ERPNext MCP integration
3. Zero human developer pipeline
4. Configure Vercel deployments for team repos

## What Works

- GitHub repos created for each team member
- ERPNext users created via API
- ClawHub skills installed
- Local embeddings for memory (nomic-embed-text)
- Google Gemini API configured as default model

## Lessons Learned

- gws/gog OAuth blocked - test users not resolving callback properly
- David prefers direct answers, no corporate filler
- Team of 6 needs agent instances
- Build toward automation - minimize human dependency

## Preferences

- No "Great question" or "I'd be happy to help" filler
- One sentence if it fits
- Can call things out directly
- Swearing allowed when it lands