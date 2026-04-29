# Local AI Gateway: nginx + YaCy over Tor

This repo packages the nginx gateway and YaCy/Tor routing that were running locally on Windows. It is meant to be a clean GitHub-ready upload: no runtime data, no private bridge credentials, and no machine-specific secrets.

## What it does

- Serves a single HTTPS landing page through nginx
- Proxies YaCy through nginx on `/yacy/`
- Sends YaCy outbound fetches through Privoxy and Tor
- Keeps the public surface small by avoiding plaintext gateway ports

## Layout

- `docker-compose.yml` - YaCy, Privoxy, and Tor orchestration
- `docker/Dockerfile.tor` - Tor container build
- `docker/Dockerfile.privoxy` - Privoxy container build
- `docker/privoxy.conf` - Privoxy forwarding config
- `docker/torrc` - Tor config without private bridge values
- `nginx/default.conf` - HTTPS reverse proxy config
- `nginx/html/index.html` - Gateway landing page
- `.env.example` - Local overrides for bind IPs and ports

## Quick start

1. Copy `.env.example` to `.env` and set the LAN bind IP you want.
2. Put TLS certs in `nginx/ssl/` as `nginx.crt` and `nginx.key`.
3. Run `docker compose up -d` from the repo root.
4. Open the nginx gateway over HTTPS and use the YaCy link from there.

## Tor notes

The included Tor config is intentionally scrubbed. If you need obfs4 bridges, add your own values locally instead of committing them to the repo.

## YaCy notes

To keep clicked result links routed through the proxy path, enable YaCy's URL proxy settings in the UI after the first launch.

## Privacy notes

- Do not commit `.env`
- Do not commit TLS private keys
- Do not commit Tor bridge credentials or other machine-specific secrets
- Keep YaCy data volumes out of the repo
