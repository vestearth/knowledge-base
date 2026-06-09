# Source Link Convention

## Purpose

Every durable claim should point back to where it came from.

This keeps the vault useful for engineering work instead of becoming a second layer of stale summaries.

## How To Use

Use this whenever a note starts making a claim that someone may need to verify later.

Do not add source boilerplate to throwaway capture items unless they are about to be promoted.

After using it, the reader should be able to trace the claim back to the repo, document, or artifact that forced it.

## Current Convention

Use both forms when a note may matter outside this machine:

```text
Source: ai-dev-office/README.md
Local: /Users/earth/Documents/GitHub/ai-dev-office/README.md
```

For quick notes, absolute local paths are acceptable. Convert them later if the note becomes reusable.

## When To Link Sources

Add source links for:

- decisions
- architecture claims
- status conclusions
- workflow rules
- prompt behavior
- reusable skills
- dashboard or analytics behavior

## When A Source Changes

If the source file changes, update the note only when the old claim becomes misleading.

Do not chase every small edit. The vault should preserve useful understanding, not mirror the repo line by line.

## Example

```text
Claim: Dashboard is read-only.
Source: ai-dev-office/dashboard/README.md
Local: /Users/earth/Documents/GitHub/ai-dev-office/dashboard/README.md
Related: [[Read-Only Dashboard Boundary]]
```

## Related

- [[Evidence Required]]
- [[Verification Loop]]
- [[Promotion Rule]]
