# Cloudflare Tunnel Status

## Status: ✅ RESOLVED (2026-04-20)

All services are routed and working via the `erpnext-geniinow` tunnel (ID: `27ab7917-df5e-484d-acb1-8c8fa4bb4ddf`).

## Current Ingress Rules

| Hostname | Destination | Status |
|----------|-------------|--------|
| erp.geniinow.com | localhost:8081 | ✅ Working |
| chat.geniinow.com | localhost:8065 | ✅ Working |
| dashboard.geniinow.com | localhost:8082 | ✅ Working |
| listmonk.geniinow.com | localhost:9000 | ✅ Working |
| cad.geniinow.com | localhost:3000 | ✅ Working |

## What Was Fixed

- **2026-04-20**: Updated `cad.geniinow.com` from `localhost:8090` → `localhost:3000` via Cloudflare API.
- Tunnel config version: `6`
- Cloudflared daemon running on Mac Studio (PID 27822, 27957)
- External test: `curl https://cad.geniinow.com` → `HTTP 200`

## API Access

- Cloudflare API token saved securely at: `~/.cloudflared/api-token.txt` (chmod 600)
- Account ID: `d245c1e6b7a7e9f95e948edaa4b60139`
- Token permissions verified: can read/write tunnel configurations

## Notes

- The local `~/.cloudflared/config.yml` should be kept in sync with remote config to avoid drift.
- If adding new services in the future, update both the remote config (via API) and local `config.yml`.
