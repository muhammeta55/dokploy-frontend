# DevOps Dokploy Frontend

## Purpose & Technologies

A Next.js (App Router, TypeScript, Tailwind CSS) frontend, containerized
with a multi-stage Dockerfile and deployed via Dokploy using Docker
Compose and Auto Deploy.

## Features

- Home page with a "Check Backend" action
- Fetches and displays the backend's root message and current version

## Running Locally

1. Install dependencies: `npm install`
2. Create `.env.local` with: NEXT_PUBLIC_BACKEND_URL=http://localhost:3000
3. Start the dev server: `npm run dev`

## Running with Docker

```bash
docker build --build-arg NEXT_PUBLIC_BACKEND_URL=https://BACKEND_DOMAIN -t dokploy-frontend .
docker run -p 3000:3000 dokploy-frontend
```

## Environment Variables

- `NEXT_PUBLIC_BACKEND_URL` – base URL of the backend API. Since this is a
  `NEXT_PUBLIC_` variable, it is embedded at build time (passed as a
  Docker build argument), not read at runtime.

## Docker Image

- Multi-stage build: a `builder` stage compiles the app (`next build`
  with `output: 'standalone'`), and a separate `runner` stage only ships
  the compiled standalone output, keeping the final image small.
- Runs as a dedicated non-root user (`appuser`), not root.
- The standalone server binds to `0.0.0.0` (via the `HOSTNAME` env var)
  so it is reachable from Traefik inside the Docker network.

## Deployment

This application is deployed via Dokploy using Docker Compose. The GitHub
repository is connected to Dokploy through a GitHub App integration.
Auto Deploy is enabled on the `main` branch: every push triggers a GitHub
webhook, which Dokploy receives and uses to automatically rebuild and
redeploy the container — no manual deployment or SSH access is used.
Domain routing and HTTPS are handled by Dokploy's built-in Traefik +
Let's Encrypt integration.

## Live Domain

https://muhammet-frontend.team-vit-devops.nl
