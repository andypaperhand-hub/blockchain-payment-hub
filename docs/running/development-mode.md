# Development Mode (Without Docker)

Use this when you are actively editing code and want instant reload. Run each service in its own terminal.

## Terminal 1 — PostgreSQL + Redis (still via Docker)

```bash
docker compose up postgres redis
```

## Terminal 2 — Backend

```bash
cd packages/backend
pnpm run start:dev
```

The backend reloads automatically when you save a file.

## Terminal 3 — Frontend

```bash
cd packages/frontend
pnpm run dev
```

Visit **http://localhost:3000**.

## Terminal 4 — Oracle service

```bash
cd packages/oracle
pnpm run start:dev
```

{% hint style="info" %}
Only PostgreSQL and Redis run in Docker here; the three Node services run locally so file changes hot-reload instantly.
{% endhint %}
