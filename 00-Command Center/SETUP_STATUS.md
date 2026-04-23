# Genii Studio Setup Status
**Last Updated:** April 2026

## Infrastructure
- [x] ERPNext v16.10.1 running at https://erp.geniinow.com
- [x] 9 companies configured (Homegenii, Genii Holdings, Genii Technologies, GolfGenii, Genii Homes, Genii Living, Genii Developments, Genii Capital, Genii AI)
- [x] 10 users created (David, Amber, Robert, Tony, John, Mark, Aryan, Antonio, Syed)
- [x] GitHub org `Genii-ai` created with 2 repos (genii-studio-knowledge, geniinow-projects)
- [x] Mattermost at https://chat.geniinow.com (all 9 humans + 10 bots active)
- [x] LM Studio running Gemma 4 e4b (local AI provider)
- [x] Ollama running (nomic-embed-text, qwen2.5:1.5b, nemotron-3-nano:30b)
- [x] Cloudflare tunnel active
- [x] GBrain knowledge base operational

## ERPNext Operational Setup — PHASE 1 COMPLETE
### Manufacturing Foundation
- [x] Manufacturing domain enabled (already active)
- [x] 10 Workstations created (Foundation through Final Inspection)
- [x] 10 Operations created with QC checkpoints
- [x] 13 Factory items created (Steel, TGI, SIP, Adhesive)
- [x] UOMs created (Each, Set, Linear Ft, Cartridge, Sq Ft)

### Custom DocTypes
- [x] Module Master (serial tracking, HCD insignia, production stages)
- [x] QC Inspection Log (A-1 through A-7 inspection types, pass/fail/conditional)
- [x] Land Acquisition (APN, zoning, status tracking)
- [x] Construction Loan (draw schedule, lender tracking)
- [x] LP Investor (capital calls, distributions, IRR)

### BOMs
- [x] MODULE-STD-001 finished good item
- [x] BOM-MODULE-STD-001-001 (12 components: steel, TGI, SIP, adhesive)

### Test Data
- [x] Module Master: HG1-00001 (Fresno, Stage 1, In Production)
- [x] QC Inspection Log: HG1-00001 Floor Assembly Pass
- [x] Land Acquisition: 123-456-78 (Fresno, Under Contract)
- [x] Construction Loan: CL-2026-00002 (Fresno Factory Build, $1.5M)
- [x] LP Investor: Schy Family Trust ($500K committed)

## In Progress
- [ ] NoVNC connectivity for cad.geniinow.com (Cloudflare WebSocket 403/1010)
- [ ] ERPNext Phase 2-7 (Role permissions, Chart of Accounts, Routing, Mattermost notifications)

## Pending
- [ ] Manufacturing route with operations linked to BOM
- [ ] Role profiles per employee
- [ ] Chart of accounts per company
- [ ] Project templates for real estate development
- [ ] Mattermost notifications (QC fails, production complete, deals)
- [ ] GBrain ingestion of all manufacturing docs

## Blockers
1. NoVNC external access (Cloudflare security policy on WebSocket)
2. CEO review of next phases

## Source Documents Ingested
- [x] Quality Control Manual (Google Drive → Genii Vault)
- [x] Modular Home Factory Budget (Google Drive → Genii Vault)
- [ ] MREA Ops Manuals (located, not ingested)
