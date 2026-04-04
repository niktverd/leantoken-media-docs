# leantoken.tech media docs

This repository contains the published documentation source for `leantoken.tech`.
The site is built with Diplodoc from `docs/` into `docs-html/`.

Production site: `https://doc.leantoken.tech`

Public docs source repository: `https://github.com/niktverd/leantoken-media-docs`

## Local development

Install dependencies:

```bash
npm ci
```

Start the local preview server with rebuilds enabled:

```bash
npm start
```

The preview server listens on `http://0.0.0.0:8077/` and redirects the root page to `http://0.0.0.0:8077/ru/index.html`.

Build the static site without starting the watcher:

```bash
npm run build:docs
```

The generated output is written to `docs-html/`.

## Deploy to Dokploy

Use a Dokploy `Application`, not `Compose`.

This repository builds into one static website and now includes a production `Dockerfile` for Dokploy. The container:

- builds the docs with `npm run build:docs`
- serves `docs-html/` with `nginx`
- redirects `/` based on the browser `Accept-Language` header, with English fallback

### Dokploy setup

1. Create a new `Application` in Dokploy.
2. Connect the Git repository and choose the branch you want to deploy.
3. Set the build type to `Dockerfile`.
4. Keep the Dockerfile path as `./Dockerfile` unless you move it.
5. Expose container port `80`.
6. Attach your domain in Dokploy and deploy.

No environment variables are required for the current production build.

Current production address: `https://doc.leantoken.tech`

## Docker build locally

Build the production image:

```bash
docker build -t leantoken-tech-media-docs .
```

Run it locally:

```bash
docker run --rm -p 8080:80 leantoken-tech-media-docs
```

Then open `http://localhost:8080/`.

## Repository notes

- Source content lives under `docs/` and `sources/projects/`.
- Public docs source repository: `https://github.com/niktverd/leantoken-media-docs`
- Published shared images live under `images/`.
- `docs-html/` is generated output and should not be edited manually.
