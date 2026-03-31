---
title: "feat: Mobile Expense Tracker Web App"
type: feat
status: completed
date: 2026-03-31
---

# Mobile Expense Tracker Web App

## Overview

A mobile-first, single-file web app for tracking personal expenses per person. Users can add people, log expenses (via quick increment or custom amount), and view expense history with running totals. Built with vanilla HTML/CSS/JS, persisted in localStorage, deployed free on GitHub Pages.

## Problem Statement / Motivation

Need a simple, always-accessible expense tracker on mobile — no app store install, no sign-up, no cost. Existing apps are bloated for this use case. A single HTML file on GitHub Pages solves this with zero friction.

## Proposed Solution

A single `index.html` file containing all HTML, CSS, and JS. Three views managed via hash-based routing. Data stored in localStorage. Mobile-first responsive CSS.

## Technical Approach

### Architecture

```
index.html (single file)
├── <style> — Mobile-first CSS, view transitions
├── <div id="app"> — View containers (show/hide via hash routing)
│   ├── #view-home — People list + add person
│   ├── #view-person — Person detail + expenses + increment button
│   └── #view-settings — Configure increment amount
└── <script> — App logic
    ├── Router — hash-based (#/, #/person/:id, #/settings)
    ├── Store — localStorage CRUD (people, expenses, settings)
    └── UI — render functions per view
```

### Routing

Hash-based routing using `window.onhashchange`:
- `#/` → Home (people list)
- `#/person/:id` → Person detail
- `#/settings` → Settings

This gives browser back/forward button support for free.

### Data Model (localStorage)

```js
// localStorage keys: "et_people", "et_expenses", "et_settings"

people: [{ id: "uuid", name: "string", createdAt: "ISO8601" }]

expenses: [{ id: "uuid", personId: "uuid", amount: 50, note: "string|null", createdAt: "ISO8601" }]

settings: { incrementAmount: 50 }
```

- IDs generated via `crypto.randomUUID()`
- Amounts are integers (whole numbers only, in ₹)
- Expenses sorted newest-first
- People sorted alphabetically

### Implementation Phases

#### Phase 1: Core Structure & Navigation

**File: `index.html`**

- [x] HTML skeleton with three view containers (`view-home`, `view-person`, `view-settings`)
- [x] Mobile-first CSS: viewport meta tag, system font stack, touch-friendly sizing (min 44px tap targets)
- [x] CSS: color scheme, card-based layout, sticky header with nav
- [x] Hash-based router: parse `location.hash`, show/hide views, handle `hashchange`
- [x] Navigation: back arrow on detail/settings views, settings gear icon on home

#### Phase 2: Data Layer (Store)

**File: `index.html` (script section)**

- [x] `Store` object with localStorage read/write helpers
- [x] `Store.getPeople()` / `Store.addPerson(name)` / `Store.deletePerson(id)`
- [x] `Store.getExpenses(personId)` / `Store.addExpense(personId, amount, note)` / `Store.deleteExpense(id)`
- [x] `Store.getSettings()` / `Store.updateSettings({ incrementAmount })`
- [x] `Store.getTotalForPerson(personId)` — sum expenses
- [x] Initialize defaults on first load (`incrementAmount: 50`)
- [x] Input validation:
  - Person name: trimmed, non-empty, max 50 chars
  - Expense amount: positive integer, max 9,99,999
  - Increment amount: positive integer, 1–9,99,999
  - Note: optional, max 200 chars

#### Phase 3: Home View (People List)

**File: `index.html` (home view section)**

- [x] Empty state: friendly message + prominent "Add Person" button
- [x] People list: card per person showing name + total (₹ formatted with commas)
- [x] Tap card → navigate to `#/person/:id`
- [x] "Add Person" button → inline form (name input + submit)
- [x] Delete person: swipe or long-press reveals delete, with confirmation dialog
- [x] Duplicate name warning (allow but warn)

#### Phase 4: Person Detail View

**File: `index.html` (person detail section)**

- [x] Header: person name + back arrow
- [x] Running total displayed prominently (₹ formatted)
- [x] **Quick increment button**: large, tappable, displays "+₹{amount}" — tapping adds expense instantly
- [x] Visual feedback on tap (brief animation/color flash)
- [x] "Add Custom Expense" button → inline form (amount input + optional note + submit)
- [x] Expense history list: each entry shows amount (₹), note (if any), date/time
- [x] Delete expense: swipe or tap delete icon, with confirmation
- [x] Empty state: "No expenses yet" message

#### Phase 5: Settings View

**File: `index.html` (settings section)**

- [x] Header: "Settings" + back arrow
- [x] Increment amount input (number field, current value pre-filled)
- [x] Save button with confirmation feedback
- [x] Input validation (positive integer only)

#### Phase 6: Polish & Deploy

- [x] Format amounts: ₹ symbol, Indian comma grouping (₹1,00,000)
- [x] Date display: relative for recent ("2 hours ago"), absolute for older ("31 Mar 2026")
- [x] Handle localStorage unavailable: show alert banner
- [x] Prevent XSS: use `textContent` exclusively, never `innerHTML` with user data
- [x] Semantic HTML + ARIA labels for accessibility
- [x] Touch feedback (`:active` states on all interactive elements)
- [x] Viewport: `<meta name="viewport" content="width=device-width, initial-scale=1">`
- [x] Theme color meta tag for mobile browser chrome
- [x] `<meta name="apple-mobile-web-app-capable" content="yes">` for iOS home screen
- [ ] Test on mobile Chrome and Safari
- [ ] Create GitHub repo, push `index.html`, enable GitHub Pages

## Acceptance Criteria

### Functional Requirements

- [x] Can add a person by name
- [x] Can delete a person (cascades to their expenses) with confirmation
- [x] Can view a person's expense history and running total
- [x] Quick increment button adds an expense of the configured amount
- [x] Can add a custom expense with amount and optional note
- [x] Can delete an individual expense with confirmation
- [x] Can configure the increment amount in settings
- [x] Data persists across page refreshes (localStorage)
- [x] Browser back/forward buttons navigate between views correctly

### Non-Functional Requirements

- [x] Works well on mobile (touch-friendly, readable, fast)
- [x] Single `index.html` file, no external dependencies
- [x] Amounts displayed in ₹ with Indian number formatting
- [x] No XSS vulnerabilities (no innerHTML with user data)
- [x] Graceful handling when localStorage is unavailable

## UI Mockup (ASCII)

```
┌─────────────────────────┐
│ Expense Tracker     ⚙️  │  ← Home
├─────────────────────────┤
│                         │
│  ┌───────────────────┐  │
│  │ Amit        ₹2,500│  │
│  └───────────────────┘  │
│  ┌───────────────────┐  │
│  │ Priya       ₹1,200│  │
│  └───────────────────┘  │
│  ┌───────────────────┐  │
│  │ Rahul         ₹800│  │
│  └───────────────────┘  │
│                         │
│  ┌───────────────────┐  │
│  │   + Add Person    │  │
│  └───────────────────┘  │
└─────────────────────────┘

┌─────────────────────────┐
│ ← Amit          ₹2,500 │  ← Person Detail
├─────────────────────────┤
│                         │
│  ┌───────────────────┐  │
│  │                   │  │
│  │     + ₹50         │  │  ← Quick increment (large button)
│  │                   │  │
│  └───────────────────┘  │
│                         │
│  [+ Add Custom Expense] │
│                         │
│  ── Expense History ──  │
│  ₹50        2 hours ago │
│  ₹200  lunch  yesterday │
│  ₹50       31 Mar 2026  │
│                         │
└─────────────────────────┘

┌─────────────────────────┐
│ ← Settings              │
├─────────────────────────┤
│                         │
│  Increment Amount       │
│  ┌─────────┐            │
│  │   50    │            │
│  └─────────┘            │
│                         │
│  ┌───────────────────┐  │
│  │      Save         │  │
│  └───────────────────┘  │
└─────────────────────────┘
```

## Dependencies & Risks

- **localStorage quota**: ~5MB limit. At ~100 bytes/expense, supports ~50,000 entries — more than enough for personal use.
- **localStorage cleared**: Browser clearing data wipes everything. Acceptable risk for v1 personal use. Future: add JSON export.
- **No offline support**: Requires network for initial page load from GitHub Pages. Subsequent use works offline since everything is in one file + localStorage (browser cache helps).
- **Single device**: Data doesn't sync between devices. Accepted constraint.

## Future Considerations (Not in v1)

- Data export/import (JSON copy to clipboard)
- "Settle" / reset a person's balance
- Multiple increment presets
- PWA with service worker for true offline
- Search/filter people list
- Expense categories

## References

- Brainstorm: `docs/brainstorms/2026-03-31-expense-tracker-brainstorm.md`
- GitHub Pages docs: https://pages.github.com/
- localStorage API: https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage
- `crypto.randomUUID()`: https://developer.mozilla.org/en-US/docs/Web/API/Crypto/randomUUID
