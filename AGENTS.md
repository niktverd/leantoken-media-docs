# AGENTS.md

This file defines the working rules for agents contributing to this repository.

## Purpose

- This repository stores media content for the `leantoken.tech` website.
- The main output is service-focused Russian content that explains what a service does, how it can be used with AI, and how to connect it through BYOK on `https://leantoken.tech`.
- This is no longer a generic documentation-template repository. Treat old template metadata and sample pages as legacy material unless they are still actively used.

## Language Rules

- Use English for all agent-facing work: plans, status updates, commit messages, PR text, review notes, and this file.
- Use Russian for all end-user content inside `docs/ru/`: Markdown pages, YAML titles, YAML link names, section names, summaries, and explanatory text.
- Keep product names, brand names, API names, and UI labels in their original spelling when translation would make them less accurate.

## Repository Structure

```text
.
|- AGENTS.md
|- package.json
|- scripts/
|  \- serve.js
|- images/
|  \- <published-doc-images>
|- sources/
|  \- projects/
|     \- <service-slug>/
|        |- README.md
|        |- notes.md
|        |- links.md
|        \- assets/
|- docs/
|  |- images -> ../images
|  \- ru/
|     |- index.yaml
|     |- toc.yaml
|     \- <service-slug>/
|        |- index.yaml
|        |- toc.yaml
|        \- byok.md
|- docs-html/
\- images/
```

## What Each Part Means

- `sources/projects/` stores collected raw materials for each service: notes, links, screenshots, drafts, exported research, and any other working inputs.
- `sources/projects/<service-slug>/` is the source-of-truth folder for everything collected about one service before publishing.
- `sources/projects/<service-slug>/assets/` is the right place for raw screenshots, saved pages, temporary exports, and working visuals collected during research.
- `images/` stores published images that are referenced by documentation pages.
- `docs/images` points to the shared `images/` folder so the docs builder can resolve published images from inside `docs/`.
- `docs/` stores the published content source used by Diplodoc.
- `docs/ru/` is the active Russian content tree for the website.
- `docs/ru/index.yaml` is the top-level landing page and must list every published service that should be visible from the main page.
- `docs/ru/toc.yaml` is the top-level sidebar navigation and must include every published service section.
- `docs/ru/<service-slug>/` contains the published content for one service.
- `docs/ru/<service-slug>/index.yaml` defines the landing page for one service section.
- `docs/ru/<service-slug>/toc.yaml` defines the sidebar navigation for one service section.
- `docs/ru/<service-slug>/*.md` contains the actual published pages for one service.
- `docs-html/` is generated build output. Do not edit it manually.

## Main Flows

There are two main workflows in this repository.

### 1. Add A New Service

When a new service is introduced:

1. Create a draft folder at `sources/projects/<service-slug>/`.
2. Use this minimum draft structure:
   - `README.md` for a short summary and current status.
   - `notes.md` for raw notes and extracted facts.
   - `links.md` for source links.
   - `assets/` for screenshots, diagrams, exports, or supporting files.
3. Create a published folder at `docs/ru/<service-slug>/`.
4. Add `index.yaml` and `toc.yaml` inside that published folder.
5. Add the initial content page, normally `byok.md`.
6. Register the service in `docs/ru/index.yaml`.
7. Register the service in `docs/ru/toc.yaml` with an `include` entry that points to `<service-slug>/toc.yaml`.
8. Build and verify the result before finishing.

Recommended initial published shape:

```text
docs/ru/<service-slug>/
|- index.yaml
|- toc.yaml
\- byok.md
```

### 2. Create Content For A Service

When the user asks for content for a service:

1. Read the full directory at `sources/projects/<service-slug>/` first.
2. Gather all stored information for that service before drafting.
3. Write or update the Russian published page in `docs/ru/<service-slug>/`.
4. The main document should explain how to add BYOK to that service.

Every service document must include:

- a clear service description
- several realistic use cases for working with AI
- a BYOK section that explains Bring Your Own Key from `https://leantoken.tech` in the context of that service
- a reusable section that explains how to get an API key on `https://leantoken.tech`

## Reusable API Key Section

When a service page requires the user to bring a key from `leantoken.tech`, add a dedicated section named `How to get an API key on leantoken.tech` or a close equivalent.

That section should usually include:

1. Open `https://leantoken.tech`.
2. Use `Sign up` if the user is new, `Log in` if the user already has an account, or `Dashboard` if the user is already authenticated.
3. Open the dashboard or API area where keys are issued.
4. Create a new API key or copy an existing one.
5. If the target service needs an endpoint in addition to the key, also copy the base URL or API URL.
6. Return to the target service and paste the key into the correct settings screen.
7. Remind the user to treat the key as a secret and rotate it if it is exposed.

Adapt the last steps to the target service. The final instruction should name the exact menu, field, or configuration file where the user pastes the key.

The reusable API key section must contain these three LeanToken screenshots in this order:

1. `images/leantoken.tech.1.landing.png` for the public landing page with `Sign up`, `Log in`, and `Dashboard`
2. `images/leantoken.tech.2.dashboard.png` for the dashboard or overview page that shows the API endpoint
3. `images/leantoken.tech.3.key-managment.png` for the `API Keys` page with the `Create key` action

Use all three images inside the section whenever the page explains how to obtain a LeanToken API key.

If the screenshots show a concrete endpoint such as `https://api.leantoken.tech/v1`, include it in the instructions for services that need an OpenAI-compatible base URL.

## Image Workflow

Use two different asset locations on purpose:

- keep raw or source-specific visuals in `sources/projects/<service-slug>/assets/`
- keep published documentation images in the repo-level `images/` folder

When an image is used in a published page:

1. Move or copy the final image into `images/`.
2. Rename it to a stable, descriptive, service-specific filename such as `<service-slug>-<topic>.png`.
3. Reference it from Markdown with a relative path that goes through `docs/images`, for example `../../images/<service-image>.png` from `docs/<lang>/<service-slug>/byok.md`.
4. Add meaningful alt text that explains what the screenshot or diagram shows.

Do not keep published image references pointed at:

- temporary download folders
- saved web page asset folders
- language-specific service folders unless the user explicitly asks for that layout
- generated output under `docs-html/`

Use screenshots intentionally:

- include images when they clarify a UI flow, settings path, API key creation step, endpoint value, or model/provider selection
- prefer a short sequence of task-relevant screenshots over dumping every captured image into the page
- if both old and current UIs matter, label them clearly so the reader understands which one is current

If a page reuses LeanToken product screenshots across multiple services, keep them in `images/` and reuse the same files instead of duplicating them per service.

The three canonical LeanToken API-key screenshots are:

- `images/leantoken.tech.1.landing.png`
- `images/leantoken.tech.2.dashboard.png`
- `images/leantoken.tech.3.key-managment.png`

## Content Standards

- Prefer one focused primary page per service before splitting content into multiple supporting pages.
- Base the published text on the collected materials in `sources/projects/<service-slug>/`. Do not invent unsupported technical details.
- Convert raw notes into clean end-user documentation. Do not paste internal scraps directly into published pages.
- Keep examples concrete. AI use cases should match the actual capabilities of the service instead of generic claims.
- Keep headings, page titles, and navigation labels aligned with the actual page content.
- If you add a published page, wire it into the relevant `toc.yaml`.
- If you want a page discoverable from a landing page, add it to the relevant `index.yaml`.
- Use relative `href` values that match the file layout.
- Keep YAML indentation clean and consistent. Broken indentation breaks navigation.
- Do not leave published pages orphaned.

## Legacy Content

- Existing template-era root pages such as `docs/ru/about.md`, `docs/ru/page.md`, and `docs/ru/anotherpage.md` are legacy remnants unless the user explicitly asks to keep or update them.
- Do not use legacy sample pages as the model for new service content.
- Prefer service folders under `docs/ru/<service-slug>/` for all new work.

## Commands

- `npm start` builds the content and runs the local development server with hot reload.
- `npm run build:docs` builds `docs/` into `docs-html/`.

## Editing Rules

- Prefer editing `sources/projects/<service-slug>/` and `docs/ru/<service-slug>/` instead of adding more root-level sample pages.
- Keep file names stable, descriptive, and service-specific.
- If you move or rename a file, update all affected `href` references in the same change.
- If you publish an image, place it in `images/` and update the Markdown reference in the same change.
- Do not manually edit generated files under `docs-html/`.
