# TREK Deploy

Open deployment template for [TREK](https://github.com/liketrek/TREK), a self-hosted collaborative travel planner.

This repository contains only deployment files. It does not include personal trip data, uploads, secrets, or a modified copy of TREK.

## What This Runs

- TREK Docker image: `mauriceboe/trek:latest`
- Local service port: `127.0.0.1:3000`
- Persistent app data: `./data`
- Persistent uploads: `./uploads`
- Optional reverse proxy: Caddy

## Quick Start

```bash
cp .env.example .env
```

Edit `.env`:

```bash
APP_URL=https://trip.example.com
ENCRYPTION_KEY=replace-with-a-real-random-key
TZ=Asia/Shanghai
```

Generate a production key:

```bash
openssl rand -hex 32
```

Start TREK:

```bash
docker compose up -d
```

Check logs:

```bash
docker compose logs -f trek
```

Open:

```text
http://localhost:3000
```

If using a domain, put Caddy, Nginx, Cloudflare Tunnel, or another reverse proxy in front of `127.0.0.1:3000`.

## Example Domain Layout

Keep the main personal website on Vercel:

```text
https://www.13ang.me
```

Run TREK on a subdomain:

```text
https://trip.13ang.me
```

## Caddy Example

Update `Caddyfile`:

```caddy
trip.13ang.me {
  encode zstd gzip
  reverse_proxy 127.0.0.1:3000
}
```

Then run Caddy on the server and point the DNS record for `trip.13ang.me` to that server.

## Cloudflare Tunnel Option

If TREK is running on a home PC or NAS, Cloudflare Tunnel can expose it without opening router ports.

The tunnel should forward:

```text
trip.13ang.me -> http://localhost:3000
```

Do not commit tunnel tokens or credentials to this repository.

## Security Notes

- Do not commit `.env`.
- Do not commit `data/` or `uploads/`.
- Change the default admin password immediately after first login.
- Use HTTPS for public access.
- Keep backups of `data/` and `uploads/`.

## License

This deployment template is MIT licensed.

TREK itself is licensed separately by its upstream project. At the time this template was created, TREK uses AGPL-3.0. If you modify TREK and provide it as a network service, review the AGPL source distribution requirements.
