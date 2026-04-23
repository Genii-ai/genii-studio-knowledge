# Decisions Log

> See [[00-Command Center/Dashboard]] for the decision queue and system context.

## 2026-04-18
- **Pipedream over gws direct**: Google OAuth is unreliable on this machine. Using Pipedream Connect for Google Workspace integration instead.
- **Obsidian for docs**: Single source of truth for team knowledge. Markdown-native, works with AI context.
- **Agent runtime TBD**: Pending decision on 24/7 daemons vs on-demand agent spawning.
- **ERPNext network issue**: Containers on mismatched Docker networks. Needs careful restart to avoid data loss.

## Related
- [[00-Command Center/Dashboard]] — Decision queue
- [[01-Projects/System Integration Plan]] — Execution plan
- [[04-Systems-Runbook]] — Infrastructure context
