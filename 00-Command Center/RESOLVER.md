# RESOLVER.md — Genii Brain Filing Rules

## Decision Tree

1. **A specific named person** → `people/`
2. **A specific organization** → `companies/`
3. **A financial transaction** → `deals/`
4. **A meeting/call record** → `meetings/`
5. **Something being actively built** → `projects/`
6. **A raw possibility** → `ideas/`
7. **A mental model or framework** → `concepts/`
8. **Prose artifact** → `writing/`
9. **Institutional strategy/ops** → `org/`
10. **Political/civic** → `civic/`
11. **Public narrative/content** → `media/`
12. **Major life program** → `programs/`
13. **Domestic operations** → `household/`
14. **Private notes** → `personal/`
15. **Hiring pipeline** → `hiring/`
16. **Reusable LLM prompt** → `prompts/`
17. **Raw data import** → `sources/`
18. **Agent deliverables** → `agent/`
19. **Unsorted / quick capture** → `inbox/`
20. **Dead / historical** → `archive/`

## Disambiguation

- **Person vs Company:** Human = people/. Organization = companies/.
- **Concept vs Idea:** Teachable framework = concept. Buildable = idea.
- **Idea vs Project:** Work started = project. No work = idea.
- **Writing vs Concepts:** Distilled = concept. Developed prose = writing.
- **Org vs Programs:** org/ = institutional knowledge. programs/ = personal priorities.
- **Household vs Personal:** PA executes = household. Private reflection = personal.

## Special

- `00-Inbox/` → Legacy Obsidian inbox. Migrate to `inbox/`.
- `01-Projects/` → Legacy projects. Migrate to `projects/`.
- `02-Agents/` → Agent configs. Keep as-is or move to `agent/`.
- `03-ERPNext/` → ERP docs. Move to `org/` or `sources/`.
- `04-Operations/` → Factory ops. Move to `org/`.
- `05-Finance/` → Financial docs. Move to `deals/` or `org/`.
- `06-Legal/` → Legal docs. Move to `org/`.
- `07-Archive/` → Legacy archive. Merge with `archive/`.
- `99-Daily-Notes/` → Daily notes. Move to `inbox/` or `meetings/`.
