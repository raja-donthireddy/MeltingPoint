# Technical Context

Engineering-practice contract for code-writing agents in this project. Every implementation must respect what's written here. If a constraint conflicts with the user's request, flag the conflict — don't silently bypass it.

## Operating Context

Every later section of this contract is written for the Operating Context stated here.

- **Users:** Anyone who needs a quick temperature conversion — they enter a value in Celsius or Fahrenheit in a web browser and see the converted value immediately.
- **Operating Scale:** Small and casual — up to a few hundred users with light traffic. All conversion runs in the browser, so load is only serving static files. Built by one developer working with AI agents, delivering in short iterations.
- **Where it runs:** A static site served from a static host (not yet chosen), opened in current desktop and mobile browsers (Chrome, Edge, Firefox, Safari) on any OS, in an ordinary anonymous browser session.
- **Credentials:** End users need none — no login, accounts or sessions. Developers use their own GitHub and Azure DevOps identities. Deployment, once wired, uses the CI pipeline's secret store, never committed secrets.
- **Alongside it:** None.
- **Not the Product's concern:** How the static host is configured and operated (DNS, TLS, CDN), and the browsers themselves — the Product targets current versions and does not work around their bugs.
- **Last confirmed:** 2026-10-08

## Overriding Principles
1. **No server calls at runtime.** All conversion happens in the browser; after the page loads it makes no network requests — no fetch/XHR, no analytics beacons, no third-party scripts or fonts from a CDN. This is the Product's defining constraint.
2. **Conversion logic is pure and separate from the UI.** `toFahrenheit` and `toCelsius` are side-effect-free functions in their own module, unit-tested on their own. Components only bind to them.
3. **Every input has defined, correct handling.** Exact formulas (°F = °C × 9/5 + 32; °C = (°F − 32) × 5/9). Empty or non-numeric input shows a clear message, never `NaN`. Values below absolute zero (−273.15 °C / −459.67 °F) are rejected. Results are rounded to 2 decimal places for display only — never in the calculation.
4. **Accessible by default.** Every input is labelled, the page is fully keyboard-operable, and it meets WCAG 2.2 AA contrast.

## Frameworks
- **Angular 22** (current stable; exact version pinned at scaffold) — client-side single-page app: standalone components, signals, zoneless change detection, built by the `@angular/build` application builder (esbuild) into a static `dist/`. No `@angular/ssr`, no SSR or hydration server.
- **TypeScript** (the version Angular 22 requires, `strict` mode on) — all application and test code.
- **Node.js 24 LTS** — build and test toolchain only; never shipped to users.

## 3rd-party tech
- **GitHub** (`raja-donthireddy/MeltingPoint`) — source control and pull requests.
- **Azure DevOps** (`EnateInternal/MeltingPoint`) — work-item board (Features, Stories, Bugs); CI pipeline that posts the required `ci` status check (not yet wired).
- **Static host** (not yet chosen) — serves the built `dist/` to production.

The running app depends on no external service.

## Packages in use
**UI / state:** `@angular/core` signals (`signal`, `computed`) bound directly to the two `<input>`s. No forms library; no RxJS beyond what Angular itself depends on.
**Unit & component tests (Tier 1):** Vitest (Angular's default test runner).
**Browser tests (Tier 2/3):** Playwright on Chromium, Firefox and WebKit.
**Accessibility:** `@axe-core/playwright`, run inside the Playwright suite.
**Lint / format:** `angular-eslint`, Prettier.
**Not allowed:** UI component kits (Angular Material, PrimeNG, …), CSS frameworks, number/units libraries, analytics or error-tracking SDKs. Any new runtime dependency needs an ADR.

## Toolchain pinning
- Policy: **exact-pinned.**
- Pinned tools: Node.js 24 LTS via `.nvmrc` and `package.json` `engines.node`; npm via `package.json` `packageManager`; every npm dependency (Angular, TypeScript, Vitest, Playwright, ESLint, Prettier, …) at an exact version with `save-exact=true` in `.npmrc`; `package-lock.json` committed; CI installs with `npm ci` only.
- Bumping: one PR per bump. Angular upgrades go through `ng update`, and Angular and TypeScript move together. A bump merges only with Tier 1 and Tier 2 green.

## Instrumentation
- No remote logging, metrics, tracing or analytics — nothing leaves the browser (Principle 1).
- Production code contains no `console.log` / `console.debug` (lint-enforced). Unexpected errors are routed through a custom Angular `ErrorHandler` to `console.error`, so they are visible in browser devtools.
- `console.debug` is allowed only during local development and must not reach a commit in production code.
- The observable record is CI: test reports, Playwright traces on failure, and the production bundle size reported on every build.

## User Feedback Approach
- Channel: in-page only — no email, notifications, toasts or pop-ups.
- The converted value updates live as the user types (no Convert button) and is announced to screen readers through an `aria-live="polite"` region.
- Validation is inline beside the field, in plain, terse language — e.g. "Enter a number." / "Below absolute zero (−273.15 °C)." No error codes, no exclamation marks.
- Copy is UK English, sentence case, with a space before unit symbols and 2 decimal places: `37.00 °C`, `98.60 °F`.

## Branching and Environments
- Model: trunk-based on `main` — PR-only, squash-merge, linear history. Feature-parented Stories branch `story/<id>-<slug>` off `feature/<feature-id>-<slug>`; parentless work (Bugs, chores) branches directly off `main`. Full rules: `CONTRIBUTING.md`.
- Branches: `main`; `feature/<feature-id>-<slug>`; `story/<id>-<slug>`; `bug/<id>-<slug>`.
- Environments:
  - **Local** — `ng serve` dev server.
  - **CI** — Azure DevOps pipeline running lint, Tier 1, the production build and Tier 2 on every PR (not yet wired).
  - **Production** — the static build of `main`, deployed to the static host (host and deploy not yet chosen). No staging or PR-preview environment.
- Protected: `main` — no direct commits, no force-push, no deletion; `ci` becomes a required check once the pipeline exists.

## Git Guardrails
- Never force-push, rewrite published history, or delete a remote branch without explicit confirmation.
- Never commit directly to `main`.
- Never bypass hooks (`git commit --no-verify`) — only Raja Donthireddy may authorise a bypass, explicitly, each time.
- Never commit secrets, `.env` files, `dist/`, `node_modules/`, or scratch/diff dumps.
- Never hand-edit `package-lock.json`; regenerate it with npm.
- Hooks in place: `.githooks/pre-commit` blocks scratch/diff dumps and, once `package.json` exists, runs `npm run lint` and `npm run format:check`. Enable once per clone with `git config core.hooksPath .githooks`.
- **HITL delivery opens and closes its own board state — nothing else will.** No skill in the HITL pipeline (`/factory-to-stories`, `/factory-check-security-design`, `/factory-triage`) sets an assignee, an iteration, or an in-progress state at creation — they deliberately leave a HITL-tagged Story, Bug, or wholly-HITL Feature untouched for a human to clear. So: **before** making any change while hand-delivering one, set `System.AssignedTo` to yourself, `System.IterationPath` to the current sprint, and `System.State` to `Active`. For a Feature, delivery starts with its first Story; while it is planned, the planning skills set its state. **When** the delivering PR merges and the work item is done, set `System.State` to `Resolved`/`Closed` yourself before ending the session. Skipping either half leaves the item unassigned, out of sprint, and looking untouched for the entire time it's actually being worked.

## Testing & the ratchet
> Seeded from the Factory standard — **opt-out, not opt-in**. Trim a line only with a deliberate, recorded decision; don't start from nothing. Full standard: <https://github.com/enateltd/factory-skills/blob/main/docs/testing-and-the-ratchet-standard.md> · the *why*: <https://github.com/enateltd/factory-skills/blob/main/docs/agentic-e2e-testing-principles.md>.
>
> **Mapping for MeltingPoint** (no LLM, no server): Tier 1 = Vitest unit tests of the pure conversion functions plus component tests, every commit, $0. Tier 2 = Playwright (with axe) against the production build served by a throwaway local static server, on Chromium, Firefox and WebKit, every PR, $0 ceiling — the "live-LLM" aspect does not apply. Tier 3 = Playwright smoke against the live production URL, once a host exists. Seams: conversion module ↔ component, and component ↔ real browser DOM (real-IO side covered by Tier 2). The platform matrix is the browser matrix above.

- **Test the contract, not the LLM.** Assert on the deterministic envelope (output shape, state transition, side-effect ordering), never on generated text or chain-of-thought. *(N/A for the LLM part — MeltingPoint has no LLM; assert on the deterministic output.)*
- **Every seam gets a real-IO test on at least one side.** Mock-on-both-sides only proves the mocks agree; defects live at integration seams, not inside modules.
- **Three tiers, each catching a class the others can't.** Tier 1 — recorded-replay (real local I/O, fakes at the seams, replayed LLM fixtures; $0, every commit); Tier 2 — live-LLM, disposable (real subprocess against a per-run throwaway substrate; bounded cost, scheduled); Tier 3 — live (real production substrate and subprocess; continuous).
- **Platform matrix is non-negotiable for OS-touching code.** Filesystem/subprocess code runs Tier 1 + Tier 2 on every OS it supports, not just the dev box's.
- **Plan test coverage at the Feature level, not the Story level.** Each Feature declares at kickoff which tiers it needs, which fixtures/substrates must exist, and a Tier-2 cost ceiling.
- **The ratchet — every Tier-3 defect pins a cheap-tier regression.** A live defect's fix lands with a Tier-1 (or Tier-2) regression that would have caught it, encoding the **class** of defect **bounded by the seam** where it surfaced — not the exact reproducer, and not a project-wide sweep. A Story that surfaced from a live defect carries that ratcheting fixture as an acceptance criterion.
- **Disposable substrate, not shared.** Tier-2 substrate is created per run and torn down; reproducibility comes from a seed (tarball/manifest/script), never persistent state.
- **Assert on observable behaviour, not internal state.** For Tier 2/3 the target is what an operator could verify from external artefacts — tracker state, commit shape, ordered log events, posted comments.
