# Google Workspace Auth — Status & Fix

**Last updated:** April 18, 2026 — 4:55 PM PT
**Updated by:** Rufio (autonomous update after David left)

---

## Current Status

| Item | Status |
|------|--------|
| Service account key file | ✅ Valid, placed at `~/.config/gcp-service-account.json` |
| Service account | `genii-agents@genii-464014.iam.gserviceaccount.com` |
| Gmail/Calendar/Drive APIs | ✅ Enabled in project genii-464014 |
| **Domain-Wide Delegation (DWD)** | ❌ **NOT enabled** — this is the blocker |
| **Scopes authorized in Admin Console** | ❌ **NOT authorized** — this is also required |
| Agent configs | ✅ Already reference the key (no update needed) |

**The service account key works**, but Google Workspace blocks it because DWD isn't configured. This is a security feature — Google requires explicit admin approval before a service account can access user data.

---

## The Fix (Two Options)

### Option A: Enable Domain-Wide Delegation (Recommended — 5 minutes)

This lets ONE service account access ALL user mailboxes. Best for the agent fleet.

#### Step 1: Enable DWD on the service account

1. Go to **https://console.cloud.google.com/iam-admin/serviceaccounts?project=genii-464014**
2. Click **"genii-agents@genii-464014.iam.gserviceaccount.com"**
3. Click **"Edit"** at the top
4. Check the box: **"Enable Google Workspace Domain-wide Delegation"**
5. Click **"Save"**

#### Step 2: Authorize scopes in Google Workspace Admin Console

1. Go to **https://admin.google.com** and sign in as a super admin
2. Navigate to: **Security → Access and data control → API controls**
3. In the "Domain-wide delegation" section, click **"Manage Domain Wide Delegation"**
4. Click **"Add new"**
5. Enter:
   - **Client ID:** `113061968611775388657`
   - **OAuth scopes (comma-separated):**
     ```
     https://www.googleapis.com/auth/gmail.readonly,https://www.googleapis.com/auth/calendar.readonly,https://www.googleapis.com/auth/drive.readonly
     ```
6. Click **"Authorize"**

**Done.** Message Rufio "DWD is enabled" and I'll test all 9 agents immediately.

---

### Option B: Per-User OAuth (Fallback — 20 minutes)

No DWD needed. Each person logs in individually. Good for privacy but annoying at scale.

```bash
# Each employee runs this in Terminal:
gws auth login --scopes "https://www.googleapis.com/auth/gmail.readonly,https://www.googleapis.com/auth/calendar.readonly,https://www.googleapis.com/auth/drive.readonly"
```

This opens a browser for each person to approve access. Rufio can then store their tokens individually.

**Good for:** If you don't have Google Workspace super admin access or don't want to enable DWD.

**Bad for:** You have to repeat this for all 9 employees (and re-do it when tokens expire).

---

## What Rufio Already Did

- Moved your downloaded key from `~/Downloads/` to `~/.config/gcp-service-account.json`
- Verified the key is valid (RSA private key present, token endpoint reachable)
- Confirmed all 9 agent configs already point to the correct key
- Confirmed all required APIs are enabled in the project
- Determined DWD is the **only** remaining blocker

---

## Technical Details (For Reference)

**Service Account:** `genii-agents@genii-464014.iam.gserviceaccount.com`
**Client ID:** `113061968611775388657`
**Project:** `genii-464014`
**Key location:** `~/.config/gcp-service-account.json`
**APIs enabled:** admin, calendar-json, drive, gmail, people, sheets, etc.

**Error you'll see without DWD:**
```
401 unauthorized_client — Client is unauthorized to retrieve access tokens
using this method, or client not authorized for any of the scopes requested.
```

---

## Next Step

**Choose Option A or B, execute it, then message Rufio.**

If you get stuck on any step, screenshot the error and send it to Rufio on Telegram.

## Related
- [[01-Projects/System Integration Plan]] — Master system integration status
- [[01-Projects/Team Rollout Plan]] — Fleet activation timeline
- [[00-Command Center/Dashboard]] — Live system status
- [[00-Command Center/Agent Fleet]] — Agent registry
