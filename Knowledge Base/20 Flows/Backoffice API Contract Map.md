# Backoffice API Contract Map

## Purpose

This note maps the admin UI surface to its likely backend contract surfaces.

It is not a full route inventory. It is a fast orientation note for answering:

- which pages are wired to real APIs
- which pages may still use mock/fallback data
- which repo should be inspected next when an admin screen looks wrong

## Source Files

- `/Users/earth/Documents/GitHub/Games-Labs-backoffice/README.md`
- `/Users/earth/Documents/GitHub/Games-Labs-backoffice/app/pages/`
- `/Users/earth/Documents/GitHub/Games-Labs-backoffice/app/composables/useAdminPageData.ts`
- `/Users/earth/Documents/GitHub/Games-Labs-backoffice/app/data/mock.ts`
- `/Users/earth/Documents/GitHub/api-gateway/README.md`
- `/Users/earth/Documents/GitHub/shared-lib/proto/admin/`
- `/Users/earth/Documents/GitHub/shared-lib/proto/missionspb/missions.proto`

## Mental Model

```text
Backoffice page
  -> composable / page fetch logic
  -> api-gateway route
  -> owning backend service or admin contract
  -> response mapped back into UI state
```

## Real API Surfaces Called Out In Repo Docs

- Games list: `GET /api/v1/admin/games`
- Game groups: `category`, `category-games`, `group/level`, `group/level-games`
- Players list: `GET /api/v1/admin/user`
- Store packages: `GET /api/v1/admin/order-packages`
- Redemption library: page-specific API surface under redemption pages

## Areas That Deserve Source Verification Before Answering

- `missions.vue` and any Daily Activities admin surface
- player detail/edit/history
- leaderboard save/edit behavior
- redemption library details
- pages known to use `app/data/mock.ts`

Rule of thumb:
- if the page works visually, that still does not prove the backend contract is fully wired
- verify `page.vue`, `useAdminPageData.ts`, and `mock.ts` together before concluding

## Repo Handoff Map

### If the issue is page behavior or display logic

Inspect:
- `Games-Labs-backoffice/app/pages/...`
- `Games-Labs-backoffice/app/composables/...`

### If the issue is route exposure or auth/header behavior

Inspect:
- `api-gateway`

### If the issue is admin contract shape

Inspect:
- `shared-lib` admin or service proto
- owning backend service repo such as Missions/Game/Provider

## Known Risk Pattern

Backoffice answers often go wrong when someone assumes:

- mock preview data means backend already supports it
- page fields imply a persisted API field exists
- gateway exposure and service support are already aligned

## Good Uses For This Note

- auditing whether a page is API-backed or mock-backed
- choosing the next repo to inspect
- keeping frontend questions tied to source-of-truth contracts

## Related

- [[API Gateway - Project Map]]
- [[Games Labs Missions - Project Map]]
- [[Shared Lib - Project Map]]
