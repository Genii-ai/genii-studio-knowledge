# Team Rollout Plan — Genii AI Agent Fleet

**Target:** All 9 employees (David, Amber, Robert, Tony, John, Mark, Antonio, Aryan, Syed)
**Timeline:** 1 week (assuming GWS auth is fixed by Day 1)
**Method:** Whole-team simultaneous rollout with tiered support

---

## Rollout Philosophy

**Don't demo. Deploy.**

The agents are already running. Instead of a "check out this cool AI thing" presentation, we hand each person their agent and make them use it for real work on Day 1.

---

## Pre-Launch Checklist

- [ ] GWS auth restored (`~/.config/gcp-service-account.json` in place)
- [ ] All agent configs updated with GWS + ERPNext credentials
- [ ] PM2 restarted and all agents showing `online`
- [ ] GBrain embeddings at 100% (35/35 chunks)
- [ ] GitHub repos initialized with starter content
- [ ] Onboarding doc reviewed (`genii-ops/docs/ONBOARDING.md`)

---

## Day 1: Activation (Monday)

### Morning — Setup Sprint (Rufio + David)

| Time | Task | Owner |
|------|------|-------|
| 8:00 AM | Fix GWS auth per `GWS Auth Setup Guide.md` | David |
| 8:30 AM | Verify GWS: `gws auth status` + test Gmail read | Rufio |
| 9:00 AM | Update all 9 agent configs with GWS tokens | Rufio |
| 9:30 AM | Restart all agents: `pm2 restart all` | Rufio |
| 10:00 AM | Verify each agent can read its assigned Gmail inbox | Rufio |
| 10:30 AM | Verify ERPNext API keys for all employees | Rufio |

### Afternoon — Team Introduction (David, 30 min)

**All-hands meeting (or async video):**

> "Everyone now has an AI assistant. It reads your email, checks your calendar, accesses ERPNext, and can help with research. It's not replacing you — it's giving you a secretary that works 24/7. Start using it today. If it breaks, tell Rufio."

**What each person gets:**
- Their agent's name (e.g., "agent-amber")
- How to talk to it (Google Chat / Slack / whatever channel)
- One specific task to try today (see Day 1 Tasks below)

### Day 1 Tasks (Assigned by Role)

| Person | Agent | Day 1 Task |
|--------|-------|-----------|
| David | agent-david | "Summarize my unread emails and flag anything urgent" |
| Amber | agent-amber | "List my meetings this week and find any conflicts" |
| Robert | agent-robert | "Find the latest architectural drawings in Drive" |
| Tony | agent-tony | "Create an ERPNext task for each active job site" |
| John | agent-john | "Research 3 new land acquisition targets in Riverside County" |
| Mark | agent-mark | "Draft follow-up emails for last week's open houses" |
| Antonio | agent-antonio | "List all pending QC inspections from ERPNext" |
| Aryan | agent-aryan | "Generate a cash flow summary for Q2 from ERPNext" |
| Syed | agent-syed | "Deploy a test landing page to Vercel" |

---

## Day 2-3: Troubleshooting (Tuesday-Wednesday)

**Rufio's job:** Monitor all agent logs, fix failures, answer questions.

**Common issues to expect:**

| Issue | Likely Cause | Fix |
|-------|-------------|-----|
| "Agent didn't respond" | GWS token expired or wrong scope | Regenerate token, verify scopes |
| "Can't find my emails" | Gmail API rate limit | Wait 60 seconds, retry |
| "ERPNext says forbidden" | User doesn't have role permissions | Check ERPNext User settings |
| "I don't know what to ask" | User needs prompting guidance | Share the "Good Prompts" list |
| "This is weird / I don't trust it" | Normal resistance | Pair them with Amber (champion) |

**Support structure:**
- **Tier 1:** Employee asks their agent (self-service)
- **Tier 2:** Employee asks Rufio (via chat or direct)
- **Tier 3:** Rufio escalates to David (only for major issues)

---

## Day 4-5: Habit Formation (Thursday-Friday)

**Goal:** Each employee uses their agent for at least 3 real tasks per day.

**Rufio proactivity:**
- Morning heartbeat: Check each agent's overnight activity
- If an agent was idle for >8 hours, ping the employee: "Your agent hasn't done anything today. Try asking it to check your calendar."
- Share wins: "Amber's agent found a calendar conflict she would've missed."

**Daily standup addition (30 seconds per person):**
> "What did your agent do for you yesterday?"

This creates social proof and competitive pressure to actually use the thing.

---

## Week 2: Capability Expansion

Once basic GWS + ERPNext usage is stable, activate advanced features per role:

| Role | Week 2 Capability |
|------|-------------------|
| David | Agent drafts investor updates, monitors lead flow |
| Amber | Agent manages ERPNext task assignments, follows up on overdue items |
| Robert | Agent reads blueprints from Drive, flags discrepancies |
| Tony | Agent creates daily job site reports from ERPNext data |
| John | Agent scrapes county records for new land listings |
| Mark | Agent generates landing page copy, deploys via GitHub |
| Antonio | Agent schedules inspections, sends reminder emails |
| Aryan | Agent reconciles bank transactions with ERPNext journal entries |
| Syed | Agent reviews code, opens PRs, handles deployments |

---

## Success Metrics

| Metric | Week 1 Target | Week 4 Target |
|--------|--------------|---------------|
| Agent uptime | >95% | >99% |
| Daily active users (9 total) | 7/9 | 9/9 |
| Tasks completed per agent / day | 3 | 10 |
| GBrain pages added | +5 | +30 |
| Time saved (self-reported) | 2 hrs/person/week | 8 hrs/person/week |

---

## Rollback Plan

If this goes badly (agents break, team revolts, etc.):

1. **Immediate:** `pm2 stop all` — agents go silent but no data lost
2. **Day 1-3:** Fix the specific issue (usually GWS or ERPNext auth)
3. **Week 1:** Restart with a pilot group (Amber + Robert only)
4. **Nuclear option:** Disable agents entirely, revert to manual workflows

**Important:** Even in rollback, GBrain vault and GitHub repos remain valuable. The knowledge doesn't disappear.

---

## Communication Templates

### Announcement to Team (Send Day 1 Morning)

```
Subject: Your New AI Assistant — Live Today

Team,

You each now have a personal AI agent that integrates with your Gmail,
Calendar, Drive, and ERPNext.

What it does:
- Reads and summarizes your emails
- Checks your calendar and finds conflicts
- Creates tasks and updates records in ERPNext
- Researches, drafts docs, and more

How to use it:
[Instructions for your chat platform]

Your first task:
[See Day 1 Tasks table above]

Questions? Ask Rufio or David.

—
David
```

### Weekly Check-in (Every Friday)

```
Subject: Agent Fleet Weekly — Wins & Blockers

Wins this week:
- [List 3 specific things agents did well]

Blockers:
- [List any issues that need fixing]

Next week focus:
- [What's being activated next]
```

---

## Post-Rollout: Scaling to Contractors

After the core 9 are stable, expand to:

| Group | When | How |
|-------|------|-----|
| Subcontractors (labor crews) | Month 2 | Telegram bots, no GWS access |
| LP Investors | Month 3 | Read-only dashboards, monthly reports |
| Property managers | Month 3 | ERPNext portal + agent for maintenance tickets |
| Sales leads | Month 4 | Chatbot on homegenii.com that creates ERPNext leads |

## Related
- [[01-Projects/System Integration Plan]] — Master integration status
- [[01-Projects/GWS Auth Setup Guide]] — GWS auth fixes
- [[00-Command Center/Dashboard]] — Live system status
- [[00-Command Center/Agent Fleet]] — Agent registry
- [[02-Team-Directory]] — Team roles and capabilities

---

*Plan version: 1.0*
*Ready to execute upon GWS auth restoration*
