# Promotion Rule

## Purpose

Promotion means moving a captured idea from raw project context into a reusable knowledge object.

Do not promote because a note looks elegant. Promote because it has reuse pressure.

## How To Use

Use this when you are deciding whether a note should stay project-specific or become reusable.

Do not apply it at note creation time unless reuse is already obvious from real work.

After using it, move the note to its new role or leave it where it is without guilt.

## Default Rule

Promote a note when either condition is true:

- used twice in the same project
- useful across one other repo, task, or agent workflow

## Promotion Paths

### Project Note -> Knowledge Unit

Use when the idea is reusable but not a formal decision.

Examples:
- [[Verification Loop]]
- [[Evidence Required]]
- [[Search First]]

### Decision -> ADR

Use when a choice has durable tradeoffs and future readers need to know why it happened.

Example:
- [[ADR-0001 Filesystem Source Of Truth]]

### Prompt Fragment -> Prompt Library

Use when a prompt shape works more than once or solves a recurring failure mode.

Target:
- [[Prompt Library]]

### Lesson -> Reusable Skill

Use when a lesson becomes a repeatable operating procedure.

Target:
- [[Reusable Skills]]

### Unresolved Tension -> Open Question

Use when a topic is not ready to decide but keeps shaping work.

Target:
- [[Open Questions]]

## Anti-Patterns

- Creating an ADR for every preference.
- Creating a skill before it has been used.
- Moving a note out of project context before the source evidence is clear.
- Treating folder placement as proof that knowledge is reusable.

## Related

- [[Inbox]]
- [[Weekly Review]]
- [[Source Link Convention]]
