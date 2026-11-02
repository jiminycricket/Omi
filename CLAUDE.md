# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Omi Card Studio** is a static, single-file web application for generating fitness/wellness tracking cards. It targets Traditional Chinese-speaking users. There is no build system, no package manager, and no tests — the entire app ships as one self-contained HTML file.

## Repository Structure

- `omi-card-studio.html` — the main application (v5.5), a single dense HTML file with all CSS and JS embedded
- `omi_nourish_protein_guide.html` — a standalone reference/guide document

Deployed via GitHub Pages from the `main` branch at `https://github.com/jiminycricket/Omi`.

## Development Workflow

Open `omi-card-studio.html` directly in a browser — no server or build step needed. To preview changes, simply refresh the page.

There are no lint, test, or build commands.

## Architecture

The app is structured as a single HTML file with three logical layers, all inline:

**CSS** (top of file): Organized by phase comments (`/* Phase 1 */`, `/* Phase 2 */`, etc.), reflecting iterative feature additions. CSS is heavily minified on a few long lines.

**HTML** (body): A two-column layout — left `.panel` for form inputs, right `.preview` for canvas output. The editor panel contains three tab sections (Daily, Weekly, Challenge Calendar) plus a Nourish nutrition setup via `<details>` disclosure widgets.

**JavaScript** (bottom of file, after `<script>`): All logic is in a single script block. Key systems:

- **State**: `state` object populated by `collect()` scanning all `[data-key]` elements; persisted to `localStorage`.
- **Canvas rendering**: Two renderers — `daily()` for the daily card and a weekly renderer (`drawReflect()`, weekly page functions) for multi-page weekly cards. Canvas is always 1080×1920px (portrait). Drawing primitives: `text()`, `line()`, `rect()`, `border()`, `checkbox()`, `progress()`, `heading()`.
- **CJK text handling**: `omiRuns()` / `omiMeasure()` / `omiDraw()` handle mixed Chinese/Latin text with per-character font switching between Inter (Latin) and Noto fonts (CJK).
- **ChallengeCalendar**: A namespace that manages multi-week challenge setup, per-day tracking, and week/day lookups. Stored in localStorage.
- **Nourish (p3)**: Phase 3 subsystem managing protein, hydration, fruit/vegetable targets. UI controlled by `p3*` IDs.
- **Photo tools**: Upload, drag-to-reposition, and crop a user photo onto the card via `getPhotoGeometry()` and canvas `drawImage`.
- **Export (p5)**: Phase 5 studio workflow — zoom controls, multi-page preview, and PNG export via `canvas.toDataURL`.

All DOM IDs are used as direct handles; there is no component framework or module system.
