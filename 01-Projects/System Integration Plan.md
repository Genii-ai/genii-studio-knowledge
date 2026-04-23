# Genii Studio - System Integration Plan

**Status: OPERATIONAL** | Last Updated: April 19, 2026

---

## Executive Summary

All core infrastructure is now **live and operational**. The complete Genii Studio stack is deployed, tunneled, and accessible via public URLs. Three major services were brought online today:

1. **Mattermost** (chat.geniinow.com) - Team communication hub
2. **Listmonk** (listmonk.geniinow.com) - Email marketing & campaign management
3. **ToolJet** (dashboard.geniinow.com) - Low-code internal tools & dashboards

ERPNext (erp.geniinow.com) was already operational and remains the central ERP backbone.

---

## Infrastructure Status

| Service | URL | Local Port | Status | Admin Access |
|---------|-----|------------|--------|--------------|
| **ERPNext** | https://erp.geniinow.com | 8081 | ✅ Operational | Administrator (existing) |
| **Mattermost** | https://chat.geniinow.com | 8065 | ✅ Operational | david / DavidMattermost2026! |
| **Listmonk** | https://listmonk.geniinow.com | 9000 | ✅ Operational | admin / GeniiListmonk2026! |
| **ToolJet** | https://dashboard.geniinow.com | 8082 | ✅ Operational | david@homegenii.com / GeniiToolJet2026! |

---

## Service Details

### Mattermost
- **Version**: 9.11.1 (Team Edition)
- **Database**: PostgreSQL 13 + Redis
- **Team**: geniinow (all 9 agents + David)
- **Channels**: general, sales, support, engineering, random, ai-agents
- **Bot Accounts**: Enabled (for agent integration)
- **Outgoing Webhooks**: Configured for sales, support, general channels

### Listmonk
- **Version**: Latest (v3.x)
- **Database**: Dedicated PostgreSQL (listmonk_db)
- **SMTP**: AWS SES configured
  - Host: email-smtp.us-east-1.amazonaws.com
  - Port: 587 (STARTTLS)
  - From: noreply@geniinow.com
- **Templates**: Default system templates operational
- **Campaigns**: Ready for creation

### ToolJet
- **Version**: Latest stable (embedded try image)
- **Database**: Embedded PostgreSQL 13
- **Workspace**: Geniinow
- **Admin**: David Schy (Super Admin)
- **Use Cases**: 
  - Agent management dashboards
  - Lead tracking interfaces
  - Campaign performance views
  - Custom admin tools for non-technical team members

---

## Networking

All services are routed through **Cloudflare Tunnel** (ID: `27ab7917-df5e-484d-acb1-8c8fa4bb4ddf`):

```
erp.geniinow.com      → localhost:8081 (ERPNext)
chat.geniinow.com     → localhost:8065 (Mattermost)
dashboard.geniinow.com → localhost:8082 (ToolJet)
listmonk.geniinow.com  → localhost:9000 (Listmonk)
```

Tunnel is managed by PM2 (`cloudflared-main`) and auto-restarts on failure.

---

## Agent Fleet Status

All 9 AI agents are running in persistent daemon mode via PM2:

| Agent | PM2 Process | Status |
|-------|-------------|--------|
| david-ai-agent | agent-david | ✅ Running |
| amber-ai-agent | agent-amber | ✅ Running |
| robert-ai-agent | agent-robert | ✅ Running |
| tony-ai-agent | agent-tony | ✅ Running |
| john-ai-agent | agent-john | ✅ Running |
| mark-ai-agent | agent-mark | ✅ Running |
| antonio-ai-agent | agent-antonio | ✅ Running |
| aryan-ai-agent | agent-aryan | ✅ Running |
| syed-ai-agent | agent-syed | ✅ Running |

---

## Next Phase: Integration & Automation

### Immediate Priorities (This Week)

1. **Listmonk ↔ ERPNext Sync**
   - Build Frappe app for bidirectional contact sync
   - Trigger campaigns from ERPNext lead status changes
   - Auto-subscribe new leads to appropriate lists

2. **Mattermost Bot Deployment**
   - Activate bot accounts for each agent
   - Configure bot tokens for programmatic messaging
   - Build notification system (ERPNext → Mattermost)

3. **ToolJet Dashboards**
   - Agent fleet monitoring dashboard
   - Lead pipeline visualization
   - Campaign performance metrics
   - ERPNext data source connection

4. **GWS Full Integration**
   - Complete Gmail/Calendar/Drive API access via service account
   - Connect all employee agents to their Google Workspace
   - Automated calendar scheduling for leads

### Medium-Term (Next 2 Weeks)

5. **Landing Page Pipeline**
   - Deploy genii-landing-pages to Vercel
   - Connect lead capture forms to ERPNext
   - A/B testing framework via ToolJet

6. **AI Sales Automation**
   - LM Studio integration for local inference
   - Automated email sequences via Listmonk
   - Lead scoring and routing

7. **Obsidian Command Center**
   - Complete documentation of all systems
   - Runbook for common operations
   - Agent configuration templates

---

## Credentials Location

All service credentials are stored in:
```
~/obsidian/genii-studio/00-Command Center/service-credentials.json
```

**IMPORTANT**: This file contains production credentials. Do not commit to Git.

---

## Architecture Diagram

```
                    Internet
                       │
              Cloudflare Tunnel
                       │
        ┌──────────────┼──────────────┐
        │              │              │
   erp.geniinow  chat.geniinow  dashboard.geniinow
        │              │              │
   ┌────┴────┐    ┌────┴────┐    ┌────┴────┐
   │ ERPNext │    │Mattermost│   │ ToolJet  │
   │  :8081  │    │  :8065  │    │  :8082   │
   └────┬────┘    └────┬────┘    └────┬────┘
        │              │              │
        └──────────────┼──────────────┘
                       │
              Mac Studio (Local)
                       │
        ┌──────────────┼──────────────┐
        │              │              │
   Listmonk      Cloudflared      Agent Fleet
   :9000          Tunnel           (PM2)
                       │
              AWS SES (SMTP)
```

---

## Emergency Contacts / Troubleshooting

| Issue | Command |
|-------|---------|
| Check all services | `docker ps` |
| Check agent status | `pm2 status` |
| Restart cloudflared | `pm2 reload cloudflared-main` |
| Restart all agents | `pm2 restart all` |
| View ERPNext logs | `docker logs erpnext-nginx-1 --tail 50` |
| View Mattermost logs | `docker logs mattermost-app --tail 50` |
| View Listmonk logs | `docker logs listmonk_app --tail 50` |
| View ToolJet logs | `docker logs tooljet --tail 50` |

---

## Notes

- All Docker containers are configured with restart policies
- PM2 manages all persistent processes (agents + cloudflared)
- Cloudflare tunnel provides HTTPS without managing certificates
- AWS SES is configured for high-deliverability email
- LM Studio is running locally for AI inference (Gemma 4 e4b)
