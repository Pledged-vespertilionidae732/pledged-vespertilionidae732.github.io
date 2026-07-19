# Squarerb — Lichess Puzzle Explorer

Browse, filter, and solve chess tactics puzzles from the Lichess puzzle
database. Fully static — React frontend, no backend, deployable to GitHub
Pages for free.

## Structure

```
data-prep/     Python script that downloads + filters the Lichess puzzle DB into JSON shards
frontend/      Vite + React app (the actual site)
```

## Quick start (with sample data)

The repo ships with 5 hand-built, verified sample puzzles so you can run the
app immediately without downloading anything:

```bash
cd frontend
npm install
npm run dev
```

## Getting the real dataset

The sample data is just 5 puzzles for local dev. To get the real dataset
(tens of thousands of puzzles across every theme), see `data-prep/README.md`.
This step needs to run somewhere with unrestricted internet access — it
downloads directly from database.lichess.org.

## How puzzles work here

Each puzzle stores:
- `fen` — position **before** the first move in `moves`
- `moves` — a UCI move list where `moves[0]` is auto-played as the "setup"
  move, then the player must find `moves[1]`, the opponent auto-replies with
  `moves[2]`, and so on. This matches the raw Lichess puzzle database format
  directly, so no conversion is needed between data-prep and the frontend.

## Stats / progress tracking

There's no backend, so "personal stats" (solved count, streaks, per-theme
accuracy) live in `localStorage` — per-browser, not synced across devices.
The Stats tab has Export/Import buttons so progress can be backed up or
moved manually. If you later want real accounts and cross-device sync,
you'd need to add an actual backend (e.g. a free-tier Render/Fly service +
a database) — the React frontend can stay on GitHub Pages and just call
that API.

## Deploying to GitHub Pages

1. Push this repo to GitHub
2. Edit `frontend/vite.config.js` — set `base: '/your-repo-name/'` to match your actual repo name
3. In your repo's Settings → Pages, set **Source** to "GitHub Actions"
4. Push to `main` — `.github/workflows/deploy.yml` builds and deploys automatically
5. Your site will be live at `https://<username>.github.io/<repo-name>/`

Manual deploy alternative (no GitHub Actions):
```bash
cd frontend
npm run build
npm run deploy   # uses gh-pages package, pushes dist/ to the gh-pages branch
```
