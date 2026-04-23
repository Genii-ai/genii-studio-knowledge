# ERPNext Implementation Plan
## Genii Studio — Factory Operations & Real Estate Development
**Date:** April 2026  
**Status:** Draft — Awaiting CEO Review  
**Prepared by:** Rufio (AI Lead Admin)

---

## Executive Summary

ERPNext v16.10.1 is live at `https://erp.geniinow.com` with **9 companies** and **10 users** configured. However, the system is currently a blank slate for operational use:
- Zero custom doctypes
- Zero projects
- Zero BOMs, workstations, or operations
- Generic construction items only (no factory-specific SIP/steel BOMs)
- Manufacturing module not configured
- No QC inspection workflow
- No real estate development tracking

This plan outlines the phased implementation to make ERPNext the single source of truth for factory production, quality control, real estate project management, and financial tracking.

---

## Phase 1: Foundation (Week 1)

### 1.1 Enable Manufacturing Module
**Why:** The core of Genii's business is building modular homes. Without manufacturing enabled, we cannot track production stages, BOMs, or work orders.

**Actions:**
- Enable Manufacturing domain for **Homegenii** and **Genii Homes** companies
- Configure default manufacturing settings (scrap %, overhead rates)
- Enable Quality module for inspection workflows

### 1.2 Create Factory-Specific Item Groups
**Current state:** Generic groups exist (Cabinetry, Electrical, etc.)  
**Gap:** No factory-level organization for SIPs, steel, prefab components

**New Groups Needed:**
```
All Item Groups
├── Raw Materials
│   ├── Steel (C-Channel, J-Lip, Bolts)
│   ├── SIP Panels (Walls, Roofs, Floors)
│   ├── Lumber (Framing, TGI/TJI)
│   └── Adhesives & Sealants
├── Prefabricated Components
│   ├── Wall Assemblies
│   ├── Floor Assemblies
│   └── Roof Assemblies
├── Subcontractor Labor
│   ├── Site Prep
│   ├── Framing
│   ├── Electrical
│   ├── Plumbing
│   ├── HVAC
│   └── Finish Work
└── Regulatory & Compliance
    ├── HCD Fees
    ├── DAA Fees
    └── Permits
```

### 1.3 Create Factory Items & UOMs
**Source:** QC Manual + Budget Form

**Critical Items to Add:**
| Item Code | Description | UOM | Category |
|-----------|-------------|-----|----------|
| STL-001 | J-Lip Steel 6"x2.5" 14ga | Linear Ft | Steel |
| STL-002 | C-Channel Steel 6"x2" 3/4 lip 14ga | Linear Ft | Steel |
| STL-003 | Bolts 3/8"x6" | Each | Steel |
| SIP-001 | 6" SIP Wall Panel R-28 | Each | SIP Panels |
| SIP-002 | 10" SIP Roof Panel R-45 | Each | SIP Panels |
| SIP-003 | SIP Splines & Connectors | Set | SIP Panels |
| TGI-001 | TGI/TJI Joist 11-7/8" | Each | Lumber |
| TGI-002 | TGI Rim Board | Each | Lumber |
| ADH-001 | PL 400 Subfloor Adhesive | Cartridge | Adhesives |

**Note:** SIP-001 through SIP-004 already exist but need verification against actual specs.

### 1.4 Create Workstations (10 Production Stages)
**Source:** QC Manual Section 8 — Module Production Flow

| Workstation | Stage | Purpose |
|-------------|-------|---------|
| WS-FOUND | Stage 1 | Foundation Stand Setup |
| WS-FLOOR | Stage 2 | Floor Assembly |
| WS-WALL | Stage 3 | Wall Assembly |
| WS-FRAME | Stage 4 | Rough Framing |
| WS-MECH-ROUGH | Stage 5 | Mechanical Rough-In |
| WS-ROOF | Stage 6 | Roof Assembly |
| WS-EXTERIOR | Stage 7 | Exterior Finish |
| WS-MECH-FINISH | Stage 8 | Mechanical Finish & Testing |
| WS-INTERIOR | Stage 9 | Interior Finish |
| WS-FINAL | Stage 10 | Final Inspection |

### 1.5 Create Operations
Each workstation gets 1+ operations:
- Foundation Stand Setup → QC Base Frame Inspection
- Floor Assembly → QC Sub Floor Inspection
- Wall Assembly → QC SIP Panel Connection Inspection
- Rough Framing → QC Framing Inspection
- Mechanical Rough-In → QC Inspection Prior to Concealment
- Roof Assembly → QC Roof Inspection
- Exterior Finish → (no QC hold)
- Mechanical Finish → QC Electrical/Plumbing Inspection
- Interior Finish → QC Drywall Inspection
- Final Inspection → QC + QAA Inspection

---

## Phase 2: Quality Control System (Week 1-2)

### 2.1 Custom DocTypes for QC
**Why:** ERPNext's built-in Quality module is generic. We need HCD-specific inspection forms tied to serial numbers.

**New DocTypes:**

#### QC Inspection Log (`QC Inspection Log`)
- **Module:** Manufacturing
- **Fields:**
  - Module Serial Number (HG1-XXXXX or HG2-XXXXX)
  - Factory Location (Fresno / Hollister)
  - Inspection Type (Incoming A-1 | In-Process A-2~A-5 | Final A-6 | Deficiency A-7)
  - Stage (1-10)
  - Inspector (User link)
  - Subcontractor (Supplier link)
  - Pass/Fail/Conditional
  - Deficiency Details (Text)
  - Corrective Action (Text)
  - Re-inspection Required (Checkbox)
  - Photos (Attach)
  - Signature (Data)
  - Date/Time

#### Module Master (`Module Master`)
- **Module:** Manufacturing
- **Fields:**
  - Serial Number (unique, auto: HG1/HG2 prefix)
  - Model Number
  - Project (link to Project)
  - Factory Location
  - Current Stage
  - Status (In Production | On Hold | QC Pass | QAA Approved | Shipped)
  - HCD Insignia Number
  - Plan Approval Number
  - Wind/Seismic Design Loads
  - Date of Manufacture

#### Subcontractor Qualification (`Subcontractor Qualification`)
- **Module:** Buying
- **Fields:**
  - Subcontractor Name (link to Supplier)
  - Specialty
  - Insurance Expiry
  - Safety Record
  - Approved (Checkbox)
  - Factory Location(s)

### 2.2 QC Workflow Automation
- **Trigger:** When QC Inspection Log = "Fail" → auto-set Module Master status = "On Hold"
- **Trigger:** When all Stage 1-9 inspections = "Pass" → auto-create Final Inspection task
- **Trigger:** When Final Inspection = "Pass" → notify QAA (2E Modular LLC)
- **Dashboard:** Real-time factory status by location

---

## Phase 3: Real Estate Development (Week 2)

### 3.1 Project Structure
**ERPNext Project module** will track each development from land acquisition to Certificate of Occupancy.

**Project Types:**
- Land Acquisition
- Entitlement & Permitting
- Site Development
- Module Production
- Site Installation
- Lease-Up / Sale

### 3.2 Custom DocTypes for Real Estate

#### Land Acquisition (`Land Acquisition`)
- **Module:** Projects
- **Fields:**
  - APN / Parcel Number
  - Address
  - County/Jurisdiction
  - Offer Price
  - Purchase Price (actual)
  - Close Date
  - Escrow
  - Status (Under Contract | Closed | Fallen Through)
  - Zoning
  - Utilities Available
  - Environmental Phase I (Checkbox + Date)
  - Project (link)

#### Construction Loan (`Construction Loan`)
- **Module:** Accounts
- **Fields:**
  - Lender
  - Loan Amount
  - Interest Rate
  - Draw Schedule
  - Project (link)
  - Status (Applied | Approved | Funded | Paid Off)

#### LP Investor (`LP Investor`)
- **Module:** Accounts
- **Fields:**
  - Investor Name
  - Commitment Amount
  - Capital Called
  - Distributions
  - Project (link)
  - IRR (Calculated)

#### Project P&L Report (`Project P&L`)
- **Module:** Projects
- **Fields:**
  - Project (link)
  - Total Revenue
  - Land Cost
  - Construction Cost
  - Soft Costs
  - Financing Cost
  - Gross Profit
  - Margin %

### 3.3 Project Template
**Standard Project Tasks (auto-generated):**
1. Site Due Diligence (30 days)
2. Entitlement & Permitting (90 days)
3. Financing Close (45 days)
4. Site Development (60 days)
5. Module Production (per unit: 1-4 weeks)
6. Site Installation (per unit: 1-2 weeks)
7. Final Inspection / CO (14 days)
8. Lease-Up / Sale (ongoing)

---

## Phase 4: BOMs & Manufacturing Routes (Week 2-3)

### 4.1 Module BOM Template
**Template:** Standard 2-Story Module

**BOM Structure:**
```
Module-STD-001 (Finished Good)
├── Stage 1: Foundation
│   ├── STL-001 J-Lip Steel
│   ├── STL-002 C-Channel
│   └── STL-003 Bolts
├── Stage 2: Floor Assembly
│   ├── TGI-001 Joists
│   ├── TGI-002 Rim Board
│   └── SIP-001 Floor Panels
├── Stage 3: Wall Assembly
│   ├── SIP-001 Wall Panels
│   └── SIP-003 Splines
├── Stage 4: Rough Framing
│   └── LUM-001 Framing Lumber
├── Stage 5: Mechanical Rough-In
│   ├── ELE-XXX Electrical
│   └── PLB-XXX Plumbing
├── Stage 6: Roof Assembly
│   └── SIP-002 Roof Panels
├── Stage 7: Exterior Finish
│   ├── EXT-XXX Siding
│   └── WIN-XXX Windows
├── Stage 8: Mechanical Finish
│   ├── HVAC-XXX Units
│   └── FIX-XXX Fixtures
├── Stage 9: Interior Finish
│   ├── DRY-XXX Drywall
│   ├── CAB-XXX Cabinets
│   └── FLR-XXX Flooring
└── Stage 10: Final
    └── Regulatory (HCD Insignia, Compliance)
```

### 4.2 Routing
Each BOM gets a routing through the 10 workstations with estimated time per operation.

---

## Phase 5: Financial Integration (Week 3)

### 5.1 Chart of Accounts Setup
**Per Company:** Homegenii, Genii Homes, Genii Developments, Genii Capital

**Required Accounts:**
- 1200 — Construction in Progress
- 1300 — Land Held for Development
- 2100 — Construction Loans Payable
- 2200 — LP Capital
- 4100 — Home Sales Revenue
- 4200 — Rental Revenue
- 5100 — Direct Construction Costs
- 5200 — Land Acquisition Costs
- 5300 — Soft Costs (Permits, Fees)

### 5.2 Cost Centers
- Factory — Fresno
- Factory — Hollister
- Development — [Project Name]
- Corporate

---

## Phase 6: User Permissions & Roles (Week 3)

### 6.1 Role Profiles
| Role | ERPNext Role | Access |
|------|--------------|--------|
| David (CEO) | System Manager + All | Full |
| Amber (Business Manager) | Manufacturing Manager + Accounts Manager | Operations, Finance |
| Robert (Draftsman/Facility) | Manufacturing User + Stock User | BOMs, Drawings, Inventory |
| Tony (Labor/Project Coord) | Manufacturing User + Projects User | Production, QC |
| John (Land/Loans) | Sales Manager + Projects User | Acquisitions, Loans |
| Mark (Realtor/Modular Sales) | Sales User + Stock User | Sales, Inventory |
| Aryan (Accountant/CFO) | Accounts Manager + Purchase Manager | Finance, Reporting |
| Antonio (QC/Facility/HCD) | Quality Manager + Manufacturing User | QC, Compliance |
| Syed (Software Engineer) | System Manager (limited) | Integrations, API |

---

## Phase 7: Integrations & Automation (Week 4)

### 7.1 GitHub Integration
- ERPNext custom app stored in `Genii-ai/geniinow-projects`
- All custom DocType JSONs version-controlled
- CI/CD for ERPNext custom app deployment

### 7.2 Mattermost Notifications
- QC Fail → Alert #qc-alerts
- Module Complete → Alert #production
- Land Acquisition Close → Alert #deals
- Daily factory status → #operations

### 7.3 GBrain Knowledge Integration
- QC Manual → Vector DB for agent queries
- Budget Form → Cost estimation prompts
- HCD Regulations → Compliance checking

---

## Risk Assessment

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| HCD changes regulations | Medium | High | Document control system with revision tracking |
| Subcontractor no-show | High | Medium | Approved list + backup per specialty |
| SIP supplier delay | Medium | High | 2-week buffer stock + alternate supplier |
| ERPNext learning curve | High | Low | Phased rollout + training docs |
| Cloudflare blocks API | Low | High | Local backup + retry logic |

---

## Success Metrics

- [ ] All 10 production stages tracked in ERPNext
- [ ] 100% of modules have QC inspection logs
- [ ] Real-time factory dashboard showing both locations
- [ ] Project P&L auto-generated per development
- [ ] All employees log into ERPNext daily
- [ ] Construction loan draws tracked against budget
- [ ] HCD compliance docs generated automatically

---

## Immediate Next Steps (This Session)

1. **CEO Review:** David reviews and approves this plan
2. **Enable Manufacturing:** Turn on Manufacturing domain for Homegenii
3. **Create Module Master DocType:** First custom doctype for serial tracking
4. **Import SIP/Steel Items:** Bulk upload factory-specific materials
5. **Set up Workstations:** Configure 10 production stage workstations

---

*Plan generated from QC Manual, Budget Form, and current ERPNext state audit.*
