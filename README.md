# LeetDuel

A daily coding competition for two people. Each player logs the LeetCode problems they solved
that day, with the problem number and title as proof, before midnight. The goal is five
problems a day. Whoever misses more days pays for the next outing.

Live: https://hemanth-uf-cs.github.io/leetduel/

## What it does

- **Logging:** each log holds the problems solved that day. Points are 1 for easy, 2 for
  medium and 3 for hard.
- **Leaderboard:** today's score, total score, current and longest streaks, and missed days.
- **Pay tab:** works out who owes an outing from the difference in missed days, minus the
  outings already paid.
- **Stats:** 30-day series, a 17-week heatmap, weekly and monthly summaries, achievements and
  CSV export.
- **Reminders:** a countdown to the midnight deadline.

## How sync works

Two people edit the same state from different devices, often offline, so writes are merged
rather than overwritten (`mergeStates` in `src/lib/storage.ts`):

- Logs are keyed by `user|date`, and the most recently updated copy wins.
- Deleting a day writes a tombstone. A log is dropped if its tombstone is newer than its last
  edit.
- A reset stores a timestamp. Anything older than it is discarded on every merge.
- Payment history is a union keyed by id, and payment counts take the maximum.

State is stored in a Firebase Realtime Database. It is also saved to `localStorage`, so the app
keeps working with no backend, or when a request fails.

## Run

```bash
npm install
npm run dev                     # local-only mode, data stays in this browser
VITE_DB_URL=<firebase-rtdb-url> npm run dev   # shared mode
npm run build
```

Pushes to `main` deploy to GitHub Pages (`.github/workflows/deploy-pages.yml`). The database
URL comes from the `VITE_DB_URL` repository secret.

## Stack

React 19, TypeScript, Vite, Recharts, date-fns, Firebase Realtime Database and GitHub Pages.
