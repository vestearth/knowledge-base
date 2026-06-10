# Auth To Wallet Missions Bootstrap Flow

## Purpose

This note captures the user bootstrap rail that starts at Auth registration and fans out to downstream services.

It exists because signup success is often mistaken for full downstream initialization, when the real behavior depends on event delivery and consumer wiring.

## Source Files

- `/Users/earth/Documents/GitHub/Games-Labs-Auth/README.md`
- `/Users/earth/Documents/GitHub/Games-Labs-Wallet/README.md`
- `/Users/earth/Documents/GitHub/Games-Labs-Missions/README.md`
- `/Users/earth/Documents/GitHub/shared-lib/README.md`

## Main Flow

```text
User registers in Auth
  -> Auth emits user-registered event / queue fanout
  -> Wallet and Missions consume bootstrap message
  -> downstream state is created or initialized
  -> later APIs assume that bootstrap state exists
```

## Ownership By Repo

- `Games-Labs-Auth`: registration and token lifecycle owner
- `Games-Labs-Wallet`: wallet-side bootstrap consumer and later spend/transfer owner
- `Games-Labs-Missions`: missions-side bootstrap consumer and reward system owner

## Runtime Dependencies

Auth docs call out:

- `RABBITMQ_URL`
- `RABBITMQ_QUEUE_USER_REGISTERED`
- `RABBITMQ_QUEUE_USER_REGISTERED_WALLET`
- `RABBITMQ_QUEUE_USER_REGISTERED_MISSIONS`

This means registration success alone is not the whole story. Downstream state depends on queue and consumer health too.

## Why This Matters

- missing wallet state can look like a balance/API bug
- missing missions state can look like a product-rule bug
- auth success can still coexist with broken downstream bootstrap

## Follow-Up Questions To Ask

1. Did Auth complete registration successfully?
2. Was RabbitMQ configured and reachable?
3. Did Wallet consume the bootstrap message?
4. Did Missions consume the bootstrap message?
5. Are later APIs failing because bootstrap state was never created?

## Related

- [[Games Labs Missions - Project Map]]
- [[Shared Lib - Project Map]]
- [[Backoffice API Contract Map]]
