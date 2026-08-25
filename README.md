# SEM2 Warroom

A private, single-file exam planner for the Semester 2 exam block (NLP, DRL, IR, ACI —
5 Sep to 13 Sep 2026). One HTML file, no build step, no dependencies, no network calls.

```
index.html      the entire app — open it and it works
serve.mjs       optional local static server (localStorage needs a real origin)
.nojekyll       tells GitHub Pages to serve the file as-is
```

## Running it

**Simplest:** double-click `index.html`. Everything works; progress is saved per browser.

**Better (recommended):** serve it over HTTP so the saved state gets a stable origin that
survives moving or renaming the file.

```bash
node serve.mjs
```

Then open <http://localhost:4321>.

## Putting it on GitHub, privately

```bash
git init && git add . && git commit -m "SEM2 warroom"
gh repo create sem2-warroom --private --source=. --push
```

Two ways to reach it from anywhere:

- **GitHub Pages** — Settings → Pages → deploy from `main` / root. Note that Pages on a
  *private* repo requires GitHub Pro; on a free account, enabling Pages makes the site
  public at `username.github.io/sem2-warroom`. The page has `noindex,nofollow` and holds no
  personal data until you type some, but treat a free-account Pages URL as public.
- **Keep it private** — clone the repo on each device and open the file locally, or use a
  Codespace. Your progress lives in the browser, not in the repo, so syncing devices means
  **Backup → export JSON → import on the other device**.

## What it does

**Today** — live countdowns to all four exams, coverage and pace stats, today's tasks with
one-click ticking, a study timer that logs sessions, and the open-book protocol card.

**Plan** — the full 25 Aug → 13 Sep schedule, day by day. Every task is editable; you can
add days, add tasks, change lecture ranges. Finished past days fold themselves away.

**Subjects** — a 16-lecture grid per subject. Each lecture tracks *watched / indexed /
revised*, a 🟢🟡🔴 confidence flag, and free-text notes (key concepts, formulas, algorithms,
timestamps). Filter to "not watched", "🔴 weak", or "not indexed".

**Index** — the `SEMESTER_OPEN_BOOK_MASTER_INDEX`, in the prescribed format:
`Topic → Subject → Lecture → Page/Timestamp → Formula/Algorithm`. Searchable across topics,
keywords, formulas and aliases; exports to Markdown or CSV. This is the thing that actually
wins an open-book exam — build it while you watch, not after.

**Stats** — progress rings, confidence map per subject, 21-day study-time chart, session log.

**Setup** — edit subjects, exam dates/times and lecture counts; set playback speed and hours
per lecture (the plan's time estimates follow); export/import backups; reset.

## Pace logic

"Plan pace" compares lectures you have watched against everything scheduled **up to
yesterday** — today is still in play, so an unfinished today never counts as being behind.
"Needed / day" divides the lectures left by the plan days left.

## Data, and not losing it

Everything is stored in your browser's `localStorage` under `sem2planner.state.v1`. Nothing
is uploaded anywhere.

- **Ctrl+Z** undoes the last change (40 steps, in memory for the session).
- Two open tabs stay in sync instead of overwriting each other.
- **Export a JSON backup** before clearing browsing data, switching browser, or changing
  device. That file is the only copy that outlives the browser profile.

## Keyboard

| Key | Action |
|---|---|
| `1`–`6` | Switch tab |
| `n` | New master-index entry |
| `/` | Search the index |
| `t` / `l` | Start-pause timer / log the session |
| `e` | Expand or collapse all plan days |
| `d` | Dark / light |
| `Ctrl+Z` | Undo |
| `?` | Shortcuts |

In the lecture grid, **click** opens a lecture and **shift-click** marks it watched. In a
plan day, **click a lecture pill** to tick that one lecture; ticking the day's checkbox
toggles the whole range.
