1# Multi-Store Platform (Next.js + Postgres + Prisma)

## Local setup (Windows / PowerShell)

### 1) Install dependencies

```bash
npm install
```

### 2) Create `.env`

This repo includes `env.example`. Copy it to `.env` and fill values:

- `DATABASE_URL` (**must** start with `postgresql://` or `postgres://`)
- `SESSION_SECRET` (any long random string)

> Note: `.env` is ignored by git and may be hidden by some tools.

### 3) Database migrations + seed

```bash
npm run db:migrate:dev
npm run db:seed
```

### 4) Start dev server

```bash
npm run dev
```

Open `http://localhost:3000`

## Demo accounts

- Admin: `admin@demo.com` / `Demo@12345`
- Merchant: `merchant@demo.com` / `Demo@12345`

## If you get Prisma error P1001 (Can't reach database)

Your `DATABASE_URL` points to a Postgres server that is not running/reachable.image.png

### Recommended: use Railway Postgres

1. In Railway, create a new project
2. Add a Postgres database
3. Copy its connection string and set it as `DATABASE_URL` in your local `.env`
4. Run migrations + seed again:

```bash
npm run db:migrate:dev
npm run db:seed
```

## Deploy to Railway (no Vercel)

### Important (repo structure)

If your GitHub repo root contains a folder named `multistore-platform/`, then in Railway you must set:

- **Root Directory**: `multistore-platform`

Otherwise Railway will fail with:
`Couldn't find any pages or app directory`.

### Steps

1. Create a new Railway project
2. Add a **PostgreSQL** database to the project
3. Add a **Node.js** service connected to your GitHub repo
4. In the service settings:
   - **Root Directory**: `multistore-platform` (only if your repo has this folder)
   - **Build Command**: `npm run build`
   - **Start Command**: `npm start`
5. Set environment variables:
   - `SESSION_SECRET` = any long random string
   - `DATABASE_URL` is usually provided automatically by Railway Postgres (verify it exists)
6. Deploy

On start, the app runs `prisma migrate deploy` automatically (see `npm start`).
