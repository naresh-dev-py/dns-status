# DNS Status | Real-Time DNS Supervision Web Dashboard

[![Releases](https://img.shields.io/github/v/release/naresh-dev-py/dns-status?label=Releases&color=blue)](https://github.com/naresh-dev-py/dns-status/releases)

![DNS Status Hero](https://images.unsplash.com/photo-1535223289827-42f1e9919769?auto=format&fit=crop&w=1400&q=80)

Badges
- [![DNS](https://img.shields.io/badge/topic-dns-blue)](https://github.com/topics/dns)
- [![TypeScript](https://img.shields.io/badge/language-TypeScript-3178c6)](https://www.typescriptlang.org/)
- [![Vite](https://img.shields.io/badge/tool-vite-646cff)](https://vitejs.dev/)
- [![TailwindCSS](https://img.shields.io/badge/style-TailwindCSS-38b2ac)](https://tailwindcss.com/)
- [![Yarn](https://img.shields.io/badge/package-yarn-2c8ebb)](https://yarnpkg.com/)
- [![OpenAPI](https://img.shields.io/badge/api-OpenAPI-ff69b4)](https://swagger.io/specification/)

Table of Contents
- About
- Key Features
- Tech Stack
- Screenshots
- Install (Local)
- Run
- CLI & Scripts
- API (OpenAPI)
- Architecture
- Deployment
- Monitoring & Logs
- Releases
- Contributing
- Roadmap
- License
- Contact
- FAQ

About
DNS Status is a lightweight web dashboard for real-time DNS supervision. It tracks DNS health, records, response time, and propagation for domains. The UI shows a live status map, historical charts, and alert triggers. The project uses TypeScript, Vite, Tailwind CSS, and a small Node.js backend. The design focuses on clarity, low latency, and readable status pages.

Key Features
- Live DNS checks for A, AAAA, CNAME, MX, NS, TXT records.
- Latency metrics and rolling averages per resolver.
- Propagation checks across multiple global resolvers.
- Health status with color-coded badges and uptime timeline.
- OpenAPI spec for the backend checks and config endpoints.
- Lightweight client built with Vite and TypeScript.
- Tailwind CSS for a compact and responsive UI.
- Local dev server with hot reload via nodemon.
- Simple alerting hooks for webhooks and email integrations.
- Export results in JSON and CSV.

Why this repo
- You need a simple status page for DNS without heavy infrastructure.
- You need a dev-friendly stack that uses modern tools.
- You want an OpenAPI-first backend and a fast frontend build.

Tech Stack
- Frontend: Vite, TypeScript, Tailwind CSS
- Backend: Node.js, TypeScript, express, nodemon
- API: OpenAPI (Swagger)
- Package manager: Yarn
- Build: Vite
- CI: GitHub Actions (examples in .github/workflows)
- Tests: Jest (unit), Playwright (e2e)
- Data store: SQLite for local, Postgres for production
- Monitoring: Prometheus style metrics endpoint

Screenshots
![Dashboard Overview](https://raw.githubusercontent.com/naresh-dev-py/dns-status/main/assets/screenshots/dashboard.png)
![Detail View](https://raw.githubusercontent.com/naresh-dev-py/dns-status/main/assets/screenshots/detail.png)

Install (Local)
Prerequisites:
- Node.js 18+
- Yarn 1.22+
- SQLite (or Postgres if you choose production)
- git

Clone
```bash
git clone https://github.com/naresh-dev-py/dns-status.git
cd dns-status
```

Install dependencies
```bash
yarn install
```

Environment
Create .env from the template:
```bash
cp .env.example .env
# Edit .env to set DB connection and API keys
```

Run
Start dev frontend and backend with one command:
```bash
yarn dev
```
- The backend runs on http://localhost:4000 by default.
- The frontend runs on http://localhost:5173 by default.

Production build
```bash
yarn build
yarn start
```

CLI & Useful Scripts
- yarn dev — run both frontend and backend in dev mode.
- yarn build — build both clients and server.
- yarn start — run built server.
- yarn test — run unit tests.
- yarn lint — run TypeScript and code style checks.

Backend quick check
To run a quick DNS check against a domain:
```bash
curl -X POST http://localhost:4000/api/check \
  -H "Content-Type: application/json" \
  -d '{"domain":"example.com","records":["A","MX"]}'
```
The API returns a JSON object with timings, resolver hits, and status.

API (OpenAPI)
The backend exposes an OpenAPI spec at /api/docs. It lists endpoints for:
- /api/check (POST) — run a DNS check
- /api/status (GET) — current aggregated status
- /api/domains (GET/POST) — list and add tracked domains
- /api/alerts (GET/POST) — list and configure alert hooks
- /metrics (GET) — Prometheus metrics

You can generate types from the OpenAPI spec to keep the client and server in sync. The project includes a script to regenerate types:
```bash
yarn gen:openapi
```

Architecture
- Frontend (Vite): fetches status and renders charts. Uses web sockets for live updates.
- Backend (Express + TypeScript): queues DNS checks, runs them against a set of resolvers, stores results, and exposes API.
- Checkers: small worker modules that run DNS queries using native resolvers or DNS-over-HTTPS when configured.
- Storage: stores checks with timestamps and raw answers. Keeps rollups for charts and uptime.
- Alerting: simple webhook sender with retry and backoff. Supports SMTP via direct config.

How the checks run
- The backend schedules checks on a cron-like interval per domain.
- It runs parallel lookups against multiple resolvers.
- Each lookup returns timings, record set, and response code.
- The system calculates health based on configured rules (e.g., any A missing => degraded).
- It stores the raw response and an aggregated result.

Deployment
Docker
- The repo includes a Dockerfile for the server and a docker-compose for full stack.
- Example:
```bash
docker-compose up --build
```

Kubernetes
- The chart folder includes a basic manifest for a Deployment and Service.
- Use a PersistentVolume for DB if you use SQLite in a pod. Prefer Postgres for production.

Cloud
- You can host the frontend as static files on any CDN.
- Host the backend on any Node server or serverless platform that supports Node.
- Use managed Postgres for persistence.

Monitoring & Logs
- The backend exposes a /metrics endpoint for Prometheus.
- Logs write to stdout in structured JSON for easy ingestion.
- Use Grafana for dashboards. Example Grafana panels are in /deploy/grafana.

Releases
Download the latest packaged release from:
https://github.com/naresh-dev-py/dns-status/releases

This link contains packaged builds and executable artifacts. Download the release file that matches your platform and execute it to run the packaged server. For example, download dns-status-v1.2.0-linux.tar.gz, extract it, and run:
```bash
tar xzf dns-status-v1.2.0-linux.tar.gz
cd dns-status-v1.2.0
./dns-status    # run the binary or follow README in the archive
```

You can also use the badge below to open the Releases page:
[![Download Releases](https://img.shields.io/github/downloads/naresh-dev-py/dns-status/total?label=Downloads&color=green)](https://github.com/naresh-dev-py/dns-status/releases)

If the download link fails, check the "Releases" section on the project page for assets and instructions.

Contributing
- Fork the repo and open a pull request.
- Follow the code style and run yarn lint before pushing.
- Write tests for new features. Use Jest for unit tests and Playwright for UI flows.
- Add API schema changes to the OpenAPI file.

Issue tracking
- Use GitHub Issues for bugs and feature requests.
- Tag issues clearly: bug, enhancement, help wanted.

Roadmap
- Enhanced propagation map with geo heatmaps.
- Native DoH and DoT support.
- More alert channels (Slack, PagerDuty).
- Role-based access and read-only public status pages.
- Mobile-friendly PWA for on-call view.

Changelog
- Follow conventional commits.
- Releases include changelog notes under the Releases page.

License
- MIT License. See LICENSE file.

Credits
- Built with Vite, Tailwind CSS, and TypeScript.
- Inspired by open source status pages and monitoring tools.
- Icons from Icons8 and Unsplash hero images under free license.

Contact
- Repo: https://github.com/naresh-dev-py/dns-status
- Use GitHub Issues to report bugs or request features.
- For security issues or sensitive data, open an issue with the "security" label.

FAQ
Q: Which DNS records do you test?
A: A, AAAA, CNAME, MX, NS, TXT. You can extend checkers in src/checkers.

Q: Can I add my own resolver list?
A: Yes. Set RESOLVERS in .env or use the API to update resolver pools.

Q: Does it support DNS-over-HTTPS?
A: Yes. Enable DOH in config and provide DoH endpoints.

Q: Where are metrics stored?
A: Short-term metrics go into the DB. Long-term metrics roll up into daily aggregates.

Q: I need a hosted version.
A: Check the Releases page for packaged builds or Docker images. Or deploy via the included manifests.

End of file