# database-container

Shared **PostgreSQL** for Nexus admin, client, and signaler. This repo holds the **container definition** and connection notes—not the data itself (data lives in the volume / Railway Postgres disk).

## Railway (recommended)

1. In your Railway project, add **PostgreSQL** (or deploy this `docker-compose` on a Railway **Docker** service if you self-host the image).
2. Open the Postgres service → **Variables** (or **Connect**).
3. Copy **`DATABASE_URL`** (Railway provides the full URL, often starting with `postgresql://` or `postgres://`).
4. Paste the **same** `DATABASE_URL` into **every** app service:
   - **deploy-admin** (admin site)
   - **deploy-client** (client site)
   - **deploy-signaler** (optional)

Also use the **same** `CHAT_DB_ENCRYPTION_KEY` on all apps if your schema uses encrypted columns (Fernet). That value is a **secret**, not a “public key.”

### SSL

If your host gives an **SSL root certificate** for Postgres, append query params to the URL or set `PGSSLROOTCERT` as your platform documents. Most Railway internal URLs work without extra files.

## Local development (Docker)

From this folder:

```bash
cp .env.example .env
# edit .env — set POSTGRES_PASSWORD etc.
docker compose up -d
```

Use in your apps:

```env
DATABASE_URL=postgresql://nexus:nexus@localhost:5432/nexus
```

(Adjust user/password/db to match `.env`.)

## Migrations

Schema is created by the Flask apps on startup (`create_all` + bootstrap). For production changes, coordinate migrations from the **application** repos; this repo only defines **where** Postgres runs and how to connect.
