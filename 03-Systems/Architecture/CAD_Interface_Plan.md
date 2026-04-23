# CAD Interface Architecture Plan
## Genii Studio — Project-Based CAD/BIM Workspace
**Date:** April 2026  
**Status:** Draft — Awaiting CEO Review

---

## Problem

Current `cad.geniinow.com` is raw noVNC → FreeCAD/Blender. No auth, no project context, no dashboard. Team lands on a desktop and has to figure out which files to open.

## Goal

A team-only web interface at `cad.geniinow.com` that:
1. **Authenticates** users (team-only access)
2. **Dashboard** shows all active projects from ERPNext
3. **New Project** wizard creates project + initializes CAD workspace
4. **Project Workspace** opens the right CAD environment with correct files loaded

---

## Proposed Architecture

### Option A: Full-Stack Web App (Recommended)

```
User → Cloudflare → cad.geniinow.com
                ↓
        [Next.js App]
           ├─ Login page (team password or Mattermost OAuth)
           ├─ Dashboard (projects from ERPNext API)
           ├─ New Project wizard (creates ERPNext project + file structure)
           └─ Project Workspace
                ├─ File browser (project CAD files)
                ├─ Specifications & requirements
                ├─ "Launch CAD" button → opens noVNC with project preloaded
                └─ Collaboration notes / comments
```

**Tech Stack:**
- **Frontend:** Next.js 15 + Tailwind (fast, modern, easy to maintain)
- **Backend:** Next.js API routes (or FastAPI if we want separation)
- **Auth:** Simple shared team password (phase 1) → Mattermost/ERPNext OAuth (phase 2)
- **Data:** ERPNext Project API (single source of truth)
- **CAD Launch:** noVNC still handles FreeCAD/Blender, but pre-loads project files
- **File Storage:** Local filesystem organized by project ID

**Pros:** Clean separation, scalable, easy to customize
**Cons:** ~2-3 days to build MVP

### Option B: ERPNext Custom Page

Build the project dashboard directly inside ERPNext as a custom page/app. "Launch CAD" button opens noVNC in new tab.

**Pros:** Single system, no separate auth
**Cons:** Limited UI flexibility, harder to build rich interfaces

### Option C: Auth Proxy + Simple Selector

Put an auth layer (nginx basic auth or simple login page) in front of noVNC. After login, show a simple project list. Click project → launch noVNC.

**Pros:** Fastest to implement (~few hours)
**Cons:** Minimal functionality, not truly integrated

---

## Recommendation: Option A (Next.js)

David wants "easy for anyone" and "dashboard of projects." Option A is the only one that delivers a proper product experience.

### Phase 1: MVP (2-3 days)
1. **Login page** — team password (shared secret, rotate monthly)
2. **Dashboard** — fetch projects from ERPNext, show status
3. **New Project** — form → creates ERPNext Project + local file folder
4. **Project Card** — specs, files list, "Open in CAD" button
5. **CAD Launch** — opens noVNC with project folder mounted

### Phase 2: Polish (1 week)
- Mattermost OAuth integration
- Real-time project status sync
- File upload/download
- Comment threads per project
- Mobile-responsive dashboard

### Phase 3: Advanced (ongoing)
- Direct FreeCAD/Blender API integration (noVNC optional)
- Version control for CAD files (Git LFS)
- Automated plan set generation triggers
- AI-assisted design review

---

## Data Flow

```
ERPNext Project Module ←→ CAD Interface
    │                        │
    │ Project created        │ Fetch projects
    │ Status updates         │ Update status
    │ Module Master links    │ Show modules
    └────────────────────────┘
           ↑
    Local File System
    /projects/{project_id}/
      ├── plans/
      ├── models/
      ├── exports/
      └── specs.md
```

---

## File Organization

```
~/geniinow-projects/cad/
├── projects/
│   ├── PROJ-2026-0001/
│   │   ├── freecad/
│   │   ├── blender/
│   │   ├── plans/
│   │   │   ├── permit_set/
│   │   │   └── manufacturing_set/
│   │   └── specs.md
│   └── PROJ-2026-0002/
│       └── ...
└── templates/
    ├── standard_module/
    └── custom_module/
```

---

## Auth Strategy (Phase 1)

Simple environment variable `CAD_TEAM_PASSWORD=GeniiStudio2026!`
- Stored in `.env` on server
- Users enter password once per session
- No user management overhead during build-out

Phase 2: Integrate with Mattermost OAuth or ERPNext session.

---

## Deployment

- **Code:** `Genii-ai/cad-interface` repo
- **Host:** Same Mac Studio (localhost:3000)
- **Expose:** Cloudflare tunnel `cad.geniinow.com` → `localhost:3000`
- **NoVNC:** Keep on separate path `cad.geniinow.com/vnc` or `vnc.geniinow.com`

---

## Next Steps (If Approved)

1. Scaffold Next.js project in `~/geniinow-projects/cad-interface/`
2. Delegate to Claude Code CLI for rapid development
3. Build login + dashboard pages
4. Integrate ERPNext project API
5. Wire noVNC launch with project context
6. Deploy via Cloudflare tunnel

---

*Prepared by Rufio. Awaiting CEO go/no-go.*
