# Running with Docker (Recommended)

Docker starts all five services with one command — the easiest way to run the full system.

## Make sure Docker Desktop is running

Open Docker Desktop and wait until it reports "Docker Desktop is running" (the whale icon in the menu bar stops animating).

## Start everything

```bash
cd blockchain-payment-hub
docker compose up --build
```

{% hint style="info" %}
The first run takes 5–10 minutes to download and build images. Later runs start in under a minute.
{% endhint %}

When you see logs like these, the system is up:

```
backend   | Payment Hub backend listening on port 3001
frontend  | Ready on http://localhost:3000
oracle    | Oracle service listening on port 3002
```

Open **http://localhost:3000** in your browser.

## Stop everything

Press `Ctrl + C`, then:

```bash
docker compose down
```

To also wipe the database volumes:

```bash
docker compose down --volumes
```

## Service ports

| Service | URL |
| --- | --- |
| Frontend | http://localhost:3000 |
| Backend API | http://localhost:3001 |
| Backend API docs (Swagger) | http://localhost:3001/api/docs |
| Oracle service | http://localhost:3002 |
