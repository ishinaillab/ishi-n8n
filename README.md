# Ishi n8n

Self-hosted n8n deployment for Ishi, intended for Hostinger VPS.

## Architecture

- n8n runs in Docker Compose.
- PostgreSQL runs as a separate container.
- n8n data and PostgreSQL data use persistent named volumes.
- The public hostname is expected to be `n8n.ishinaillab.com`.
- TLS termination / reverse proxy should be handled by Hostinger's proxy layer or another dedicated reverse proxy.
- Secrets are supplied through a local `.env` file and are never committed.

## Quick start

1. Copy `.env.example` to `.env`.
2. Set strong values for:
   - `POSTGRES_PASSWORD`
   - `N8N_ENCRYPTION_KEY`
3. Confirm `N8N_HOST=n8n.ishinaillab.com`.
4. Start:
   ```bash
   docker compose pull
   docker compose up -d
   ```
5. Verify:
   ```bash
   docker compose ps
   docker compose logs --tail=100 n8n
   ```

## Hostinger deployment

Use this repository as the deployment source on the Hostinger VPS.

Recommended path:

```text
/opt/ishi-n8n
```

Clone:

```bash
git clone https://github.com/ishinaillab/ishi-n8n.git /opt/ishi-n8n
cd /opt/ishi-n8n
cp .env.example .env
```

Then edit `.env` locally on the server and start the stack.

Do not commit the real `.env`.

## Updates

This repository pins n8n to a specific tested release instead of using `latest`.

To update:
1. Change `N8N_VERSION` in `.env` or `.env.example`.
2. Review n8n release notes and breaking changes.
3. Back up volumes/database.
4. Run:
   ```bash
   docker compose pull
   docker compose up -d
   ```

## Backups

At minimum, back up:
- PostgreSQL database
- n8n persistent data volume
- the server-side `.env`

The `N8N_ENCRYPTION_KEY` must be preserved. Losing it can make stored n8n credentials unusable.
