# DevOps Dokploy Frontend

## Purpose & Technologies

A Next.js (App Router, TypeScript, Tailwind CSS) frontend, containerized
with a multi-stage Dockerfile and deployed via Dokploy using Docker Compose
and Auto Deploy.

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
  `NEXT_PUBLIC_` variable, it is embedded at build time (passed as a Docker
  build argument), not read at runtime.

## Deployment

This application is deployed via Dokploy, using a multi-stage Dockerfile
(build stage compiles the app, production stage only ships the compiled
`standalone` output). The GitHub repository is connected to Dokploy through
a GitHub App integration, with Auto Deploy enabled on the `main` branch —
every push automatically triggers a rebuild and redeploy. Domain routing
and HTTPS are handled by Dokploy's built-in Traefik + Let's Encrypt
integration.

## Live Domain

_To be added once configured in Dokploy._
