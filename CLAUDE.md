# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

@AGENTS.md

## Project

Arcade Vault is an online platform for playing retro-style browser games and competing for the highest score. The UI language is Spanish (copy, route names, component file names in the prototype).

The project follows **Spec Driven Design** using the `/spec` and `/spec-impl` skills from [Klerith/fernando-skills](https://github.com/Klerith/fernando-skills) (`npx skills@latest add Klerith/fernando-skills`). Features should be specified first, then implemented from the spec.

## Commands

- `npm run dev`: dev server at http://localhost:3000
- `npm run build`: production build (also type-checks)
- `npm run lint`: ESLint (flat config, `eslint-config-next` core-web-vitals + typescript)

No test runner is configured yet.

## Stack

Next.js 16 (App Router, `app/`), React 19, TypeScript strict, Tailwind CSS v4 via `@tailwindcss/postcss` (configured in `app/globals.css`, no `tailwind.config`). Path alias `@/*` maps to the repo root. As AGENTS.md says, check `node_modules/next/dist/docs/` before using Next.js APIs; e.g. the root layout uses the global `LayoutProps<"/">` type helper.

## Current state: the design prototype is the source of truth

`app/` is still the create-next-app scaffold. The intended product lives as a standalone HTML/JSX prototype in `resources/templates/` (open `Arcade Vault.html` in a browser; it loads React 18 UMD + Babel standalone from a CDN). It is reference material to port into the Next.js app, not code to import:

- **Screens**: `biblioteca.jsx` (game library with category filter), `detalle.jsx` (game detail), `reproductor.jsx` (game player with HUD: score/lives/level, pause, game over, save score), `auth.jsx` (login), `salon.jsx` (hall of fame / leaderboards), `nav.jsx` (top nav).
- **Routing**: `app.jsx` holds a `route` object (`{ name: "biblioteca" | "detalle" | "player" | "auth" | "salon", id? }`) serialized into `location.hash`. These map naturally onto App Router routes.
- **Data**: `data.jsx` has mock `GAMES` (id, title, short/long description, `cat`, `cover` CSS class, accent `color`, `best`, `plays`), `CATS`, and `seededScores()` for fake leaderboards. Users and submitted scores are persisted only in `localStorage` (`av_user`, `av_scores`). The game itself is simulated (score ticks on a timer); no real games exist yet.
- **Modules** share state via `window.*` globals because there is no bundler. Replace with real imports when porting.
- **Styling**: `styles.css` defines the neon/CRT theme as CSS custom properties on `:root` (`--bg*`, `--ink*`, `--cyan`, `--magenta`, `--yellow`, `--green`, medal colors `--gold/--silver/--bronze`, `--pixel` = "Press Start 2P", `--mono` = JetBrains Mono) plus BEM-ish `av-*` classes and `cover-*` game cover art. Keep these tokens when building the Tailwind theme.
