# AI Office Agency - Prompt Library

This is an index of prompt surfaces, not a place to paste every full prompt. Copy only reusable fragments or decisions that are worth reusing outside the source file.

## Source Prompt Surfaces

- Role prompts: `/Users/earth/Documents/GitHub/ai-dev-office/agents/`
- Codex-first routing: `/Users/earth/Documents/GitHub/ai-dev-office/model-routing-codex-first.md`
- Role prompt templates: `/Users/earth/Documents/GitHub/ai-dev-office/role-prompt-templates-codex-first.md`
- Skill entrypoint: `/Users/earth/Documents/GitHub/ai-dev-office/SKILL.md`
- Cursor docs and templates: `/Users/earth/Documents/GitHub/ai-dev-office/docs/cursor.md`, `/Users/earth/Documents/GitHub/ai-dev-office/docs/cursor-templates.md`
- Claude and Gemini advisory docs: `/Users/earth/Documents/GitHub/ai-dev-office/docs/claude.md`, `/Users/earth/Documents/GitHub/ai-dev-office/docs/gemini.md`

## Prompt Families

### PM Prompting

Purpose:
- turn user requests into scoped tasks
- define acceptance criteria
- identify blockers and split work when useful

Source:
- `/Users/earth/Documents/GitHub/ai-dev-office/agents/pm.md`

### Dev And Dev-2 Prompting

Purpose:
- implement scoped changes
- produce focused verification evidence
- support parallel work only when PM splits subtasks clearly

Sources:
- `/Users/earth/Documents/GitHub/ai-dev-office/agents/dev.md`
- `/Users/earth/Documents/GitHub/ai-dev-office/agents/dev-2.md`

### Reviewer Prompting

Purpose:
- review for regressions, compatibility, release risk, and missing verification
- decide whether to approve, request changes, escalate, or route infra issues

Source:
- `/Users/earth/Documents/GitHub/ai-dev-office/agents/reviewer.md`

### Debugger Prompting

Purpose:
- identify root cause
- recommend minimal-risk fixes
- route back to reviewer, dev/dev-2, or free-roam

Source:
- `/Users/earth/Documents/GitHub/ai-dev-office/agents/debugger.md`

### Free-Roam Prompting

Purpose:
- resolve ambiguity
- handle repeated blockers
- split, reroute, fix, or abort

Source:
- `/Users/earth/Documents/GitHub/ai-dev-office/agents/free-roam.md`

## Reusable Prompt Principles

- Include task artifact paths, current status, acceptance criteria, and blockers.
- Require evidence from build/tests or source inspection.
- Keep advisory model outputs non-official until normalized into artifacts.
- Avoid repeated calls for formatting-only edits.
- Escalate when blockers repeat or risk exceeds a single pass.

Source:
- `/Users/earth/Documents/GitHub/ai-dev-office/model-routing-codex-first.md`

## Prompt Candidates To Extract Later

- PM handoff prompt
- Reviewer release-risk checklist
- Debugger RCA prompt
- Manual advisory lane prompt
- Dashboard analytics review prompt
- Obsidian note extraction prompt

