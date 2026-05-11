# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**MEMOトレ** (MEMOtore) is a Japanese-language Progressive Web App (PWA) for tracking gym workouts. It is a single-file, no-build vanilla HTML/CSS/JS app with offline support via a Service Worker.

## File Format Convention

All source files use a `.md` extension (e.g., `index.html.md`, `sw.js.md`, `manifest.json.md`). Each file starts with a markdown heading comment indicating the actual filename (e.g., `# index.html`), followed by the real file content. When deploying, strip the first line and rename files to their true extensions.

## Architecture

The entire application lives in `index.html.md`. There is no framework, no bundler, and no dependencies beyond Chart.js loaded from CDN.

**Data persistence** uses two `localStorage` keys:
- `workout_ex_final_pwa` — JSON object mapping body part names to arrays of exercise names
- `workout_logs_final_pwa` — JSON array of log entries, each with `{ id, ts, date, ex, part, sets: [{w, r}] }`

**Key application state** (module-level JS variables):
- `currentPart` — active body part (e.g., `'胸'`)
- `currentMode` — training mode: `'hypertrophy'` | `'diet'` | `'health'`
- `currentDate` — `Date` used for calendar navigation
- `myChart` — Chart.js instance (destroyed and recreated on each history tab visit)

**UI sections** (toggled via `view-section` / `active` CSS classes):
- `#view-record` — main workout logging screen
- `#view-calendar` — monthly calendar with dot indicators for logged days
- `#view-history` — Chart.js line chart of total daily load + log list

**Static AI knowledge base** (`aiKnowledgeBase`) maps each body part to a hardcoded array of exercise names. The "AI recommendation" feature is entirely client-side, randomly sampling from this list.

**1RM formula**: `weight × (1 + reps / 30)` — displayed live per set.

**Training modes** (`coach` object) change the primary color CSS variable (`--primary`), the advice text, and chart colors. Colors: hypertrophy=`#2196F3`, diet=`#E91E63`, health=`#4CAF50`.

## PWA Files

- `sw.js.md` — caches `index.html` and `manifest.json` at install time; serves from cache with network fallback
- `manifest.json.md` — PWA metadata; references `icon-192.png` and `icon-512.png` (not currently in repo)
