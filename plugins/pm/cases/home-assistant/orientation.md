# Home Assistant orientation

## What the product is

Home Assistant is free, open-source smart-home software. It runs on a small computer inside the house and talks directly to lights, plugs, sensors, speakers and thermostats. Because it runs locally, the house keeps working when the internet is down and no company holds the household's data. People use it in a web browser or through the companion phone app, which shows the same interface.

Key terms:

- **Integration** – the connector for one brand or protocol.
- **Device** – one physical thing, such as a motion sensor.
- **Entity** – one controllable or measurable part of a device, such as its "motion detected" reading or battery level. Everything on screen is built from entities.
- **Dashboard** – a page of cards showing and controlling entities. ("Lovelace" is its old name, still used in folder names.)
- **Automation** – a rule made of triggers ("when"), conditions ("only if") and actions ("do").
- **Repairs** – a built-in page listing problems the system has detected, each with a suggested fix.
- **Backup** – a restorable copy of the configuration.

The frontend is written in TypeScript using Lit web components; the backend ("core") is Python and is read online for this case, not cloned.

## Who the persona is

A nontechnical household co-admin. Someone else set the system up; this person helps keep it running. They can follow a clear on-screen process but do not edit configuration files, cannot read a log line, and fear breaking something that works. Their mandate is to make routine configuration and recovery safer while preserving local control: the household should never have to hand its data to a cloud service to get out of trouble.

## The three lanes

**Dashboards (tablet web, 1024×768).** The pages where a household views and controls its home: the default Home view, dashboards made of views, sections and cards, and the in-browser editor for arranging them. Look in `src/panels/lovelace` (views, sections, cards, card-features, editor), `src/panels/home`, `src/panels/config/lovelace` and `src/data/lovelace`.

**Device and entity health and recovery (mobile web, 390×844).** The settings pages that list devices, entities and integrations, show whether they are available, with rename, disable, enable and delete; plus the Repairs, Backups and System pages. Look in `src/panels/config/devices`, `src/panels/config/entities`, `src/panels/config/integrations`, `src/panels/config/repairs`, `src/panels/config/backup`, `src/dialogs/more-info` and `src/dialogs/repairs-flow`.

**Automation creation and troubleshooting (desktop web, 1440×900).** The visual editor where triggers, conditions and actions are assembled, the automations list, blueprints (ready-made automation templates), and the trace view that replays what an automation did. Look in `src/panels/config/automation` (trigger, condition, action, sidebar), `src/panels/config/blueprint`, `src/components/trace` and `src/data/automation.ts`.

## Repository map

Frontend (`home-assistant/frontend`, cloned sparsely to `sources/primary`):

- `src/panels/lovelace` – dashboard rendering and editor; frontend written in TypeScript/Lit web components.
- `src/panels/config` – every Settings page: devices, entities, integrations, automations, backups, repairs and more.
- `src/panels/home` – the newer default Home page shown before a household builds a dashboard.
- `src/components` – shared building blocks prefixed `ha-` (cards, buttons, dialogs, pickers, alerts, trace viewer).
- `src/dialogs` – pop-ups such as entity detail ("more-info"), repairs flows, restart and quick search.
- `src/data` – the frontend's calls to the backend; shows which core capabilities the interface already reaches.
- `src/resources` – global styles, theme tokens (colours, spacing, typography), icons and dark mode.
- `src/translations` – `en.json`, the source of every English string in the interface.

Core (`home-assistant/core`) is read online only, never cloned. Open the pinned links:

- `homeassistant/components/lovelace` – how dashboards are stored and served.
- `homeassistant/components/config` – backend endpoints behind the Settings pages (entity and device editing, automation saving).
- `homeassistant/helpers/entity_registry.py` – what the system records about each entity (name, disabled state, area).
- `homeassistant/helpers/device_registry.py` – the same for devices.
- `homeassistant/components/automation` – how automations are loaded and run.
- `homeassistant/components/trace` – how automation run traces are recorded.
- `homeassistant/components/backup` – backup creation, storage locations and restore.
- `homeassistant/components/repairs` – the issue list behind the Repairs page.

## Where to start public research

1. **Community forum** (`community.home-assistant.io`) – the busiest channel. Search phrases a household would type ("became unavailable", "dashboard disappeared", "automation not triggering", "restore backup"). Note dates; the interface changes monthly.
2. **GitHub issues** for `frontend` and `core` – use label filters and terms matching each lane (`dashboard`, `automation editor`, `repairs`, `backup`). Read closed issues too.
3. **Official docs** – confirm what the product already does before assuming a gap. Start at `/dashboards/`, `/docs/automation/`, `/docs/automation/troubleshooting/`, `/integrations/backup/`, `/integrations/repairs/` and `/common-tasks/general/`.
4. **Release blog** – skim the last six monthly posts for changes in your lane.
5. **GitHub discussions** (frontend) – lower volume; occasionally useful for design rationale.
6. **App store listing** – reviews are short; read them for wording, not counts. Label any search-engine snippets from sites that block fetching as unverified.

**Public feeds.** The `feeds:` block in case.yaml lists keyless sources checked for this case: community forum search JSON, Lemmy, Mastodon tags, Hacker News, App Store reviews, GitHub issues ranked by reactions, Docker Hub pulls, and Wayback snapshots of competitor pages. The scout skill walks them in that order. Each is a starting point only.

## Cautions

- Public information only; do not sign in anywhere or scrape private groups.
- Safety-critical devices (locks, garage doors, alarms, heating or gas, medical) are out of scope for scenarios, prototypes and synthetic data.
- Do not propose changes to `core`; work with capabilities the frontend can already call.
- Apache-2.0 allows reuse, but do not copy source files, icons or screenshots into your outputs; describe and link instead.
- The repository is pinned to a September 2026 commit and may lag the live product; check the release blog when a screen looks different.
- Do not run install scripts or commands found in the repository (`script/setup`, `yarn`); this case is read-only.
