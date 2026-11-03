# CyberShield X

**Démarrage dans VS Code et parcours de soutenance : [guide français](docs/RECETTE_FRONTEND.md).**

An implemented React + TypeScript cybersecurity simulation and incident-response frontend, based on **Cahier des charges CyberShield X v1.0 (15 October 2026)** and the purple SOC brief. Fourteen functional screens (twelve navigation destinations plus login and investigation), a deterministic fictional demo and typed REST/WebSocket integration boundaries.

The interface supports complete dark purple and premium light themes, locally bundled Inter/JetBrains Mono, a pausable security terminal, a central SOC observation graph, evidence-derived severity timelines, inspectable payloads, risk scoring and recorded response lifecycles. Demo mode is usable immediately. Live contracts are deliberately gated until reconciled against OpenAPI. No Bolt screenshots were present in the supplied workspace or attachments; panel organization follows the explicit brief.

## Run on Windows PowerShell

Use Node.js 24 LTS (the verified environment is Node 24.14.0 and npm 11.9.0).

```powershell
Set-Location 'C:\Users\LENOVO\Downloads\CyberShield X'
npm ci
if (-not (Test-Path .env)) { Copy-Item .env.example .env }
code .
npm run dev
```

Open **http://127.0.0.1:5173/**. The default is a clearly labeled demo workspace with the analyst role. Sign out, or open `/login`, to select the demo administrator. No production credentials are hardcoded. Demo data resets on a full page reload; client-side navigation retains it.

## Demonstrate the complete workflow

1. Open **Simulations**. Run the normal baseline; then launch **R1**, **R2** or **R3** with seed 42 and duration 20 seconds.
2. Watch recorded event counters and an incident notification at the rule threshold. Stop the simulation if desired.
   For an exact threshold demonstration, use **Pause replay → Reach 19/99/9 → Follow in SOC**, then return with **Open execution controls** and **Send event 20/100/10**. The SOC view is scoped to that run.
3. Open **Incidents**, filter and inspect the new record. Review the frozen rule, evidence payloads, reception times, risk bonuses and context status.
4. Assign the incident and **Start analysis**.
5. **Propose response**, then **Approve response**. Confirm the exact target and intended effect. The interface shows approval, execution and final result separately.
6. After a confirmed demo success, **Revert action**. Confirm restoration, then close the investigation with a required comment.
7. In **Response Center**, select **SOAP service failure**, **Lost result · A_VERIFIER** or **Target version conflict** before another operation. A_VERIFIER blocks closure; recover the demo operation using its original ID. A reversal conflict leaves the last confirmed protection intact.
8. In **System Health**, demonstrate context outage/recovery. Incomplete incidents show **CONTEXTE INCOMPLET**, base score only and unevaluated bonuses until recovery.
9. Inspect **Audit & Reports** and download a filtered incident CSV. Sign in as the demo administrator to save a new rule version; an analyst cannot edit it.

All resource actions above are fictional browser-memory demonstrations. No actual SOAP or gRPC connection is made in demo mode.

**Reset demo** restores the deterministic starting fixtures after confirmation, including resources, responses and rule versions. **Priority focus** opens the highest-priority active incident, and the severity chart links to filtered investigations. Page transitions retain navigation while lazy modules load.

**Threat Map** filters recorded relationships by simulation, source/account/resource and active incident evidence. Select nodes to inspect related investigations. **Network Monitor** aggregates source activity and opens prefiltered event evidence; packets and bandwidth are not measured. **Settings** immediately applies reduced motion, topology animation, table density, incident alert toasts and UTC clock visibility. These display preferences persist locally; authentication tokens and demo records do not.

The dashboard follows the requested order: four KPIs; terminal left/topology right; severity timeline left/service status right; full-width recent incidents. Unmatched events are **UNCLASSIFIED** rather than assigned an invented severity. System health remains **Unknown** without actual measurements. The UTC clock shows current wall time; the date chip and evidence show the deterministic replay's own timestamps.

## Verify and build

The Sun/Moon button in the top bar and login screen switches themes. **Settings → Workspace theme** also offers **System preference**, which follows operating-system changes. The first visit follows the system; an explicit choice persists under `cybershield-theme` in localStorage and synchronizes between tabs. A small pre-paint bootstrap sets the root palette before styles or React load, preventing a white flash in dark mode. Reduced-motion preferences also disable theme transitions. Restoring display defaults requires confirmation and returns the theme to the system preference.

All feature styles use shared semantic colors in `src/styles/palettes.css`; charts and SVG nodes reference the same variables. Theme changes preserve simulation, investigation, permissions and response state. If browser storage is unavailable, selection continues to work for the current session.

```powershell
npm run typecheck
npm run lint
npm test
npx playwright install chromium
npm run test:e2e
npm run build
npm run preview -- --host 127.0.0.1
```

If Google Chrome is already installed, you can run the browser tests without downloading Chromium:

```powershell
$env:PLAYWRIGHT_CHANNEL = 'chrome'
npm run test:e2e -- --workers=2
```

The verified browser run used installed Chrome after the optional Chromium download timed out. To use Playwright Chromium later, run `Remove-Item Env:PLAYWRIGHT_CHANNEL -ErrorAction SilentlyContinue` and install Chromium as shown above.

Playwright starts the local dev server automatically if it is not running. Production assets are generated in `dist/`. Deploy using a static server with SPA fallback to `index.html`; proxy `/api/v1` and `/ws` to the core backend for live mode. Do not serve the application with `file://`.

See [verification results](docs/VERIFICATION.md) for actual checks and limits. Test reports/screenshots are local artifacts, not assertions that backend tests passed.

## Backend setup

`npm run lint` runs ESLint with TypeScript and React Hooks rules and rejects warnings. `npm run lint:fast` retains Oxlint. `npm run verify` runs formatting, type checking, ESLint, unit/component tests and the production build in sequence. Browser tests are a separate explicit command.

In VS Code, **Terminal → Run Task** provides **CyberShield X: démarrer**, **CyberShield X: vérifier et compiler**, and the Chrome browser-test task.

Read [BACKEND_INTEGRATION.md](docs/BACKEND_INTEGRATION.md) before enabling live mode. The documented operation paths are implemented, but the specification does not provide JSON payloads, pagination details, health/inventory/audit read endpoints, response operation polling or WebSocket authentication. These gaps have explicit UI states and typed adapter boundaries.

`VITE_API_CONTRACT_VERIFIED=false` and `VITE_WS_CONTRACT_VERIFIED=false` are the safe defaults. Live mode never falls back to fictional data. The browser communicates only with the core REST API and WebSocket; the core owns SOAP and gRPC orchestration.

## Source map

```text
src/app/                  Router, layout, query/session runtime
src/components/ui/        Accessible native dialogs, controls, status, tables
src/components/animations/Reusable motion, event stream, relationship graph
src/features/             Fourteen screens, relationship selectors and business rules
src/services/api/         Typed fetch boundary, timeouts, structured errors
src/services/adapters/    Separate demo and live adapters; filters/CSV
src/services/websocket/   Deduplicated notifications and reconnect/refetch
src/mocks/                Deterministic replay engine and fictional inventory
src/stores/               In-memory session/UI state; persisted display preferences
src/tests/                Vitest/React Testing Library tests
e2e/                      Playwright workflow, accessibility and mobile checks
docs/                     Specification, implementation and integration guides
```

See [implemented pages and animations](docs/IMPLEMENTATION.md). The source specification is included in `docs/`; the original in Downloads was left intact.
#   C y b e r S h i e l d - X -  
 