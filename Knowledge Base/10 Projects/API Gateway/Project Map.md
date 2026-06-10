# API Gateway - Project Map

## Purpose

`api-gateway` is the external HTTP entrypoint for the Games Labs microservice set.

It is the translation layer between public/client-facing HTTP access and the internal service graph. When a mobile, backoffice, or web contract looks wrong, this repo is one of the first places to inspect together with the owning service proto.

## Source Files

- `/Users/earth/Documents/GitHub/api-gateway/README.md`
- `/Users/earth/Documents/GitHub/api-gateway/cmd/main.go`
- `/Users/earth/Documents/GitHub/api-gateway/config/`
- `/Users/earth/Documents/GitHub/api-gateway/gateway/`
- `/Users/earth/Documents/GitHub/api-gateway/middleware/`
- `/Users/earth/Documents/GitHub/api-gateway/services/`
- `/Users/earth/Documents/GitHub/shared-lib/proto/`
- `/Users/earth/Documents/GitHub/AGENTS.md`

## Main Areas

- server bootstrap in `cmd/`
- config and service URLs in `config/`
- routing/proxy behavior in `gateway/`
- auth, validation, CORS, metrics, and error middleware
- service client management and downstream communication

## Mental Model

```text
Client / Backoffice / Mobile
  -> api-gateway HTTP route
  -> auth / validation / middleware
  -> downstream service client or grpc-gateway mapping
  -> response returned to caller
```

## Operational Role

- single public HTTP entry for internal services
- auth and policy enforcement boundary
- route ownership index for service APIs
- one of the key places where shared-lib proto changes become visible externally

## What To Check When Contracts Drift

1. Which service actually owns the endpoint?
2. Is the source contract defined in `shared-lib/proto/*`?
3. Does gateway mapping still match the proto/service behavior?
4. Is the caller reading a route that exists only in docs or mock code?

## Common Evidence Surfaces

- service URL config such as `AUTH_API_URL`, `WALLET_API_URL`, `GAME_API_URL`, `GAME_HTTP_URL`
- middleware behavior for auth/rate-limit/CORS
- route/proxy setup under `gateway/`
- downstream client code under `services/`

## Notes To Remember

- External HTTP should go through gateway, not directly to internal services.
- When a contract changes, gateway mappings and docs need to move with it.
- For Games Labs work, do not assume “service supports it” means “gateway already exposes it”.

## Good Uses For This Note

- tracing an HTTP endpoint to its owning service
- checking where auth/validation happens
- reviewing whether a frontend/mobile issue is really a gateway mapping issue
- understanding which env var controls which downstream integration

## Related

- [[Shared Lib - Project Map]]
- [[Backoffice API Contract Map]]
- [[Games Labs Missions - Project Map]]

