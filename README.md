# SEM2 Warroom

A single-file exam planner for the Semester 2 exam block (NLP, DRL, IR, ACI —
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

## Live site

**<https://rahul24-06.github.io/SEM2_WarRoom/>** — served by GitHub Pages from `main`.

The repo is public, because GitHub Pages cannot publish from a private repo on a Free plan
(and even on Pro, a personal-account Pages site is publicly reachable — auth-gated Pages is
Enterprise Cloud only).

**This does not publish your studying.** Progress, lecture notes, confidence flags and the
master index live in your browser's `localStorage`, never in this repo. What is public is
the empty planner shell — the schedule skeleton and the code. The page is also marked
`noindex, nofollow` so it stays out of search results.

If you ever want it genuinely private *and* online, the options are GitHub Pro + Enterprise
access control, or a host with password protection (Cloudflare Pages / Netlify).

To publish an update:

```bash
git add -A && git commit -m "..." && git push
```

Pages rebuilds automatically, usually within a minute.

## What it does

**Today** — live countdowns to all four exams, coverage and pace stats, today's tasks with
one-click ticking, a study timer that logs sessions, and the open-book protocol card.

**Plan** — the full 25 Aug → 13 Sep schedule, day by day. Every task is editable; you can
add days, add tasks, change lecture ranges. Finished past days fold themselves away.

**Subjects** — a 16-lecture grid per subject. Each lecture tracks *watched / indexed /
revised*, a 🟢🟡🔴 confidence flag, and free-text notes (key concepts, formulas, algorithms,
timestamps). Filter to "not watched", "🔴 weak", "not indexed", or "⚡ marks at risk". Each
cell carries a bar showing what that lecture is worth in the paper.

**Index** — the `SEMESTER_OPEN_BOOK_MASTER_INDEX`, in the prescribed format:
`Topic → Subject → Lecture → Page/Timestamp → Formula/Algorithm`. Searchable across topics,
keywords, formulas and aliases; exports to Markdown or CSV. This is the thing that actually
wins an open-book exam — build it while you watch, not after.

**Stats** — progress rings, confidence map per subject, 21-day study-time chart, session log.

**Setup** — edit subjects, exam dates/times and lecture counts; map exam weighting; connect
cross-device sync; set playback speed and hours per lecture (the plan's time estimates
follow); export/import backups; reset.

## Exam weighting — where the marks actually are

Midsem syllabus is **CS1–CS8**; the full course is **CS1–CS16**. The announced share of the
end-semester paper drawn from the midsem half:

| Subject | Midsem half | Per lecture, CS1–CS8 | Per lecture, CS9–CS16 | Verdict |
|---|---|---|---|---|
| DRL | 50% | 6.25% | 6.25% | even — treat both halves alike |
| IR | 20% | 2.5% | **10%** | a post-midsem lecture is worth **4×** |
| ACI | 10% | 1.25% | **11.25%** | a post-midsem lecture is worth **9×** |

**NLP** is weighted by the announced comprehensive structure (40 marks total):

| Modules | Marks |
|---|---|
| M2 Vector semantics · M3 Language Models + Neural LM | 5 |
| M5 POS Tagging · M6 Hidden Markov Models | 5 |
| M9 Parsing/statistical · M10 Dependency Parsing | 6 |
| M11 Contextual Embedding | 6 |
| M12 Word sense & WordNet · M13 Semantic web ontology | 6 |
| M14 RAG | 6 |
| M15 Text summarization | 6 |

These seven rows are pre-loaded in **Setup → Exam weighting** with their marks, but their
lecture numbers are deliberately **left blank**: module numbers are not assumed to equal CS
numbers. Type the CS/lecture numbers into each row (`2-3`, `9, 10`, ranges or lists both
work) and the planner starts weighting NLP. Until then NLP is simply unweighted, and the
subject header shows an amber "unmapped" warning. Any lecture that ends up in no block is
dimmed and struck through — not on the paper.

**Today → ⚡ Where the marks are** ranks every weighted lecture by *marks × how shaky you
are* (unwatched counts full, 🔴 60%, 🟡 30%, watched-but-unrated 20%, 🟢 nothing), tie-broken
by whichever exam is sooner. That list is the answer to "what do I open next".

## Pace logic

"Plan pace" compares lectures you have watched against everything scheduled **up to
yesterday** — today is still in play, so an unfinished today never counts as being behind.
"Needed / day" divides the lectures left by the plan days left.

## Sync across devices

State lives in `localStorage`, which is **per browser, per device** — that is why opening the
Pages URL on a second device shows an empty planner. The repo only ever holds the empty
shell; your progress is never committed to it.

To make devices share: **Setup → Sync across devices**, paste a GitHub token, press Connect.
The planner then mirrors everything into **one private gist** (`sem2-warroom.json`), pushing
a few seconds after each change and pulling on load and whenever a tab regains focus.
Newest change wins. On a second device you only need the token — it finds the gist itself.

Making the token: GitHub → Settings → Developer settings → **Personal access tokens →
Tokens (classic)** → tick **only the `gist` scope** → set an expiry past 13 Sep.

Know what you are handing over:

- Gists require a **classic** token; fine-grained tokens cannot access gists at all.
- A classic `gist` token can read and write **every gist on your account**, not just this one.
- It is stored in that browser only — never in the repo, never in a JSON backup, never inside
  the gist. This page is served from a public repo, so anyone with access to that browser
  profile has the token.
- **Revoke it at <https://github.com/settings/tokens> once exams are over.**

The header dot shows sync state: ○ off · ◌ working · ● synced · ▲ error (hover for why).

## Data, and not losing it

Everything is stored in your browser's `localStorage` under `sem2planner.state.v1` (the sync
token lives separately under `sem2planner.gh`, so backups never contain it).

- **Ctrl+Z** undoes the last change (40 steps, in memory for the session).
- Two open tabs stay in sync instead of overwriting each other.
- **Export a JSON backup** before clearing browsing data, switching browser, or changing
  device. With sync off, that file is the only copy that outlives the browser profile.

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
