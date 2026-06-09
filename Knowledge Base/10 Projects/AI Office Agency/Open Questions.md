# AI Office Agency - Open Questions

Use this as the holding area for unresolved design pressure. Do not force these into ADRs until a real task needs the decision.

## Active Questions

### Rules vs Skills Boundary

When should behavior live in framework rules, target project rules, reusable skills, or role prompts?

Current signal:
- Framework rules stay generic in `/Users/earth/Documents/GitHub/ai-dev-office/AGENTS.md`
- Reusable agent behavior likely belongs outside project-specific orchestration
- Role-specific execution contracts live in `/Users/earth/Documents/GitHub/ai-dev-office/agents/`

### Skill Validator Design

What should validate a reusable skill: structure only, examples, runtime dry-run, or task-level acceptance evidence?

Current signal:
- AI Dev Office already validates YAML handoffs through schemas and `validate-yaml.rb`
- Skill validation may need a different contract because skills are behavioral, not only data-shaped

### Central Skill Registry

Should the registry live inside AI Dev Office, in a separate `ai-skills` repo, or as an index consumed by multiple systems?

Current signal:
- AI Dev Office owns orchestration and handoffs
- Reusable cross-agent guidance should not become project-specific framework policy

### Obsidian Sync Strategy

Should vault notes copy summaries from project files, link to source files only, or maintain generated indexes?

Default for now:
- Notes can summarize, but every claim should link back to source files or task artifacts.
- Avoid generated indexes until manual notes reveal what is worth indexing.

### Dashboard Analytics Next Step

Should analytics remain multiple read-only endpoints, or move toward a consolidated overview/read model?

Current signal:
- Dashboard README says panels still fetch separate endpoints and there is no consolidated initial overview fetch for Analytics yet.

Source:
- `/Users/earth/Documents/GitHub/ai-dev-office/dashboard/README.md`

## Parking Lot

- How should old `runs/TASK-*` history be summarized into durable knowledge?
- Which task artifacts should become ADRs?
- Should Obsidian notes store absolute paths, repo-relative paths, or both?
- Should project maps be hand-written, generated, or hybrid?

