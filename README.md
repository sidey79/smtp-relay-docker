# smtp-relay-docker

Reusable SMTP relay stack for internal services.

## Included

- `docker-compose.yml` with a dedicated SMTP relay service (`boky/postfix`)
- `.env.example` as secret-free template
- `.gitignore` to keep local secret files out of git

## Networking model

- No host ports are published.
- Relay service is attached to:
  - internal network `relay_internal`
  - shared network `smtp_relay_net` (small dedicated subnet)
- Other stacks can send mail to host `smtp-relay` on port `25` over `smtp_relay_net`.
- Default network size is configurable via:
  - `SMTP_RELAY_SUBNET` (default `172.30.40.0/27`)
  - `SMTP_RELAY_GATEWAY` (default `172.30.40.1`)

## Quick start

```bash
cp .env.example .env
# set real relay credentials in .env
docker compose pull
docker compose up -d
```

## Security notes

- Never commit `.env`.
- Keep relay credentials only in local `.env` or secrets management.
- Restrict which containers can reach `smtp_relay_net`.
