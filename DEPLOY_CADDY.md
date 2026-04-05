# Caddy Hosting Setup

This project can be hosted with Docker Compose and Caddy as the public reverse proxy.

## What this setup does

- serves the site on your domain over HTTPS
- automatically gets and renews TLS certificates with Caddy
- forwards website traffic to the Next.js frontend container
- keeps backend off the public internet except through the app flow
- forwards uploaded file requests directly to the backend

## Files added

- `Caddyfile`
- `docker-compose.prod.yml`
- `.env.caddy.example`

## Server requirements

- Ubuntu 22.04 or 24.04 VPS
- Docker and Docker Compose installed
- Domain DNS pointed to your server public IP
- Ports `80` and `443` open in the firewall / cloud security group

## 1. Point your domain

Create DNS records:

- `A` record for `example.com` -> your server IP
- optional `A` record for `www.example.com` -> your server IP

## 2. Prepare env files

Create a production Caddy env file from the example:

```powershell
Copy-Item .env.caddy.example .env.caddy
```

Set your real domain inside `.env.caddy`:

```env
DOMAIN=yourdomain.com
LETSENCRYPT_EMAIL=you@yourdomain.com
```

Also update backend allowed origins in `BACKEND1_PROD/.env` for production:

```env
NODE_ENV=production
FRONTEND_URL=https://yourdomain.com
FRONTEND_URLS=https://yourdomain.com,https://www.yourdomain.com
```

## 3. Start production

Run:

```powershell
docker compose -f docker-compose.prod.yml --env-file .env.caddy up -d --build
```

## 4. Check status

Run:

```powershell
docker compose -f docker-compose.prod.yml ps
```

To view logs:

```powershell
docker compose -f docker-compose.prod.yml logs -f caddy
docker compose -f docker-compose.prod.yml logs -f frontend
docker compose -f docker-compose.prod.yml logs -f backend
```

## How routing works

- `/` -> frontend container on port `3000`
- `/api/proxy/*` -> frontend Next.js proxy route
- `/uploads/*` and `/api/uploads/*` -> backend container on port `5000`

## Notes

- This setup preserves your current frontend API strategy using `/api/proxy`
- If you later want a cleaner production shape, we can switch frontend calls from `/api/proxy` to `/api` and let Caddy route API traffic directly to the backend
- Your current `.env` contains live secrets; rotate them before deploying publicly
