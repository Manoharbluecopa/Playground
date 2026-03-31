# Expense Tracker Mobile Web App

**Date:** 2026-03-31
**Status:** Brainstorm

## What We're Building

A simple, mobile-first web app for personal expense tracking per person. Core features:

1. **Add People** — Maintain a list of people to track expenses for
2. **Add Expense** — Log expense entries against a specific person
3. **Quick Increment Button** — A single tap button that adds a preconfigured amount as an expense for the selected person (e.g., always +50)
4. **Configurable Increment Amount** — App-level setting to change the quick-add amount
5. **Expense History** — View logged expenses per person with a running total

Data persists in browser localStorage (single-device use).

## Why This Approach

**Vanilla HTML/CSS/JS deployed to GitHub Pages**

- The scope is small and well-defined — no complex state management or routing needed
- Single HTML file means zero build tooling, zero dependencies
- GitHub Pages provides free, reliable hosting with no config beyond pushing to a repo
- localStorage is sufficient since data only needs to live on one device
- Mobile-first CSS ensures it works well as a phone app

Rejected alternatives:
- **React/Preact + Vite** — Overkill for this scope. Adds build step, Node.js dependency, and deployment complexity for no meaningful benefit.
- **Cloud database (Firebase/Supabase)** — Not needed since the app is for single-device personal use.

## Key Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Tech stack | Vanilla HTML/CSS/JS | Simplest possible, no build step |
| Data storage | localStorage | Single device, no backend needed |
| Hosting | GitHub Pages | Free, zero config |
| Increment button | Single configurable amount | Keep UI simple, one tap = one expense entry |
| Expense model | Per-person expense entries | Each increment/manual add creates a discrete record |

## App Screens / Views

1. **Home / People List** — Shows all added people with their total expense. Tap a person to see details.
2. **Person Detail** — Shows expense history for that person. Has the quick increment button and option to add custom expense.
3. **Settings** — Configure the increment amount.

## Data Model (localStorage)

- **people**: `[{ id, name, createdAt }]`
- **expenses**: `[{ id, personId, amount, note?, createdAt }]`
- **settings**: `{ incrementAmount: 50 }`

## Deployment

1. Create a GitHub repo
2. Push the `index.html` file
3. Enable GitHub Pages from repo settings (serve from main branch)
4. Access via `https://<username>.github.io/<repo-name>/` on phone

## Open Questions

_None — all questions resolved during brainstorming._
