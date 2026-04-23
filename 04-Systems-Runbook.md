# Systems Runbook

> See [[00-Command Center/Dashboard]] for live status and [[00-Command Center/Agent Fleet]] for PM2 details.

## Infrastructure
- **ERPNext**: Docker @ localhost:8081 (v16.10.1)
- **Ollama**: Local LLM server (nomic-embed, qwen2.5, nemotron)
- **LM Studio**: Gemma 4 E4B @ http://127.0.0.1:1234
- **GitHub**: davidmschy
- **Pipedream**: CLI installed, needs workspace auth
- **OpenClaw**: Gateway running (pid 1025)

## Deployment Patterns
- Landing pages: Vercel (TBD)
- Internal tools: GitHub → self-hosted or Vercel
- Automation: Pipedream workflows
- Documentation: Obsidian + GitHub

## Adding a New Agent
1. Create `~/.openclaw/agents/{name}/`
2. Write `SOUL.md` with role, tools, permissions
3. Add to OpenClaw config
4. Grant ERPNext + Google Workspace access
5. Test with a simple task

## Related
- [[00-Command Center/Dashboard]] — System status board
- [[00-Command Center/Agent Fleet]] — Agent registry and config paths
- [[01-Projects/System Integration Plan]] — Integration status
- [[01-Projects/GWS Auth Setup Guide]] — Google Workspace auth details
