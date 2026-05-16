# Bracket Battle — Technical Brief

**Live app:** [ehall4444.github.io/bracket-battle](https://ehall4444.github.io/bracket-battle)  
**Repository:** [github.com/EHall4444/bracket-battle](https://github.com/EHall4444/bracket-battle)

---

## What It Is

Bracket Battle is a lightweight, annual bracket prediction competition built for the NCAA Division I Men's Lacrosse Championship. Players register, fill out a complete 15-pick bracket across all four tournament rounds before play begins, then track their results against real game outcomes over three weekends in May.

It was built to be simple to use on mobile, easy to maintain year over year, and fun for a small group of friends competing against each other.

---

## How It Works — User Flow

1. Players register with a username and password before the published deadline
2. They fill out picks across all four rounds (First Round → Quarterfinals → Semifinals → Championship) and save their bracket
3. At the deadline, all brackets lock — no further changes accepted
4. As tournament games are played, scores and results are entered via a password-protected admin panel
5. The leaderboard updates in real time as results come in
6. Players can browse all four round tabs to see how their picks compare to actual matchups, with color-coded correct/incorrect indicators

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML, CSS, JavaScript — single `index.html` file |
| Hosting | GitHub Pages (static, no build step) |
| Database | Supabase (PostgreSQL via REST API) |
| Auth | Custom username/password with SHA-256 hashing, no third-party auth |
| Score data | Cloudflare Worker proxying NCAA game data, with manual admin fallback |
| Comments | Supabase real-time polling (30s interval) with reactions |

---

## Architecture

The entire application is a single `index.html` file with no framework, no bundler, and no dependencies beyond two Google Fonts. All logic — auth, routing, rendering, scoring — is written in vanilla JavaScript.

**Why no framework?** Simplicity and longevity. The app needs to be maintainable by one person year over year with minimal tooling overhead. A single file is easy to audit, deploy, and roll back.

### Database Schema (Supabase)

| Table | Purpose |
|---|---|
| `users` | Username + SHA-256 password hash |
| `picks` | One row per player — full bracket stored as JSON |
| `scores` | Game-by-game scores and status, keyed by game ID |
| `comments` | Trash talk — username, body, timestamp |
| `comment_reactions` | Per-user thumbs up/down on comments |
| `pick_changes` | Audit log of every bracket save with timestamp and minutes before deadline |

### Data Model

The bracket is represented as a flat JavaScript object:

```js
{
  fr1, fr2, fr3, fr4, fr5, fr6, fr7, fr8,  // First Round picks (team keys)
  qf1, qf2, qf3, qf4,                        // Quarterfinal picks
  sf1, sf2,                                   // Semifinal picks
  fin                                          // Championship pick
}
```

Each value is a team key string (e.g. `"princeton"`, `"notredame"`). The full bracket is stored as a single JSON column in the `picks` table — one row per player.

### Tournament Structure

Games are hardcoded in JavaScript arrays (`FR`, `QF`, `SF`, `FIN`) with:
- Team matchups for First Round and Quarterfinals (set by the NCAA selection committee)
- Location, date/time, and broadcast network per game
- `result` field hardcoded for completed rounds; populated by the admin panel for rounds in progress

**QF slot mapping** is sequential by First Round game order — `qf1` = winners of `fr1`+`fr2`, `qf2` = winners of `fr3`+`fr4`, etc. This matches the order players saw when filling out their brackets and must be preserved when updating for future years.

Semifinal and Championship real matchups are only revealed in the UI once the prior round's results are confirmed — both feeding games must have a `result` before their card shows real team names. Until then, cards display "Awaiting results..." placeholders.

### Pick Validation (`sanitizePicks`)

On login, picks are loaded from the database and passed through `sanitizePicks()`, which validates that each round's picks are consistent with the prior round (e.g. your QF pick must be a team you picked to win their FR game). This prevents stale picks from corrupting later rounds during the pre-deadline picking phase.

**After the deadline**, `sanitizePicks` is skipped entirely — picks are loaded exactly as saved. This is critical: running validation post-deadline would incorrectly null out picks that were valid when saved.

### Card Footer — "Your Bracket" Display

After the deadline, every card across all four rounds shows a **"Your bracket"** footer row:

- **First Round:** `Your bracket: N. Carolina` — single team name, color-coded blue if they won, red if they lost, neutral if game not yet played
- **Quarterfinals, Semifinals, Championship:** `Your bracket: Team A v. Team B` — both predicted teams shown, each color-coded blue if they advanced to this round, red if they didn't

No points are shown on cards. The leaderboard owns all scoring display. The footer is purely a reference — players can see their predictions at a glance as results come in.

An info button (ⓘ) opens a dropdown showing pre-game odds and who else picked which team (visible after games start).

### Scoring Engine

Scores are calculated client-side from the picks object and game result fields. One pick per game, one score per round — the same model used by ESPN, Yahoo, and CBS:

- **+10/15/20/25 pts** for correct picks by round
- Wrong picks score nothing
- **Tiebreaker:** number of correctly called upsets (lower seed or unseeded team winning)

All scoring is displayed exclusively on the leaderboard. Cards show bracket picks and color-coded results only — no point totals.

### How to Play Modal

A **"? How to Play"** link in the app nav opens a modal explaining the scoring system, tiebreaker, and bracket mechanics. Accessible from any tab at any time. Intended for new players or anyone who needs a refresher mid-tournament.

### Score Ingestion

A Cloudflare Worker proxies NCAA game data and returns structured JSON. The app polls this on a smart interval — every 15 seconds when games are live, every 60 seconds otherwise. Manual score entry via the admin panel serves as a fallback when the automated feed is unavailable or incomplete.

### Auth

No third-party authentication. Passwords are hashed with SHA-256 in the browser before transmission and stored as hex strings. Sessions are persisted via `localStorage` with a "Remember me" option. There is no password reset — by design, for a small trusted group.

### Admin Panel

Accessible only to a hardcoded admin username. Allows manual entry of scores and game status (scheduled, in-progress quarter, Final) for any game across all four rounds. Changes write directly to the `scores` Supabase table and update the UI immediately.

---

## Deployment

Deployment is a direct push to the `main` branch of the GitHub repository. GitHub Pages serves the static file with no build step. Changes are live within ~60 seconds of a push.

**Rollback:** revert to any previous commit via `git revert` or GitHub's UI.

---

## Annual Reset

The app is designed to be revived each May for the same event. Items that require updating each year:

- Team data (`T` object) — names, records, seeds, odds, mascot logos
- Game structure (`FR`, `QF`, `SF`, `FIN` arrays) — matchups, locations, times, broadcast info
- Deadline and tournament end dates (`DEADLINE`, `TOURNAMENT_END` constants)
- Hero and auth background photos (optional)

The database has a `season` INTEGER column on all tables. Each year, ensure new rows are written with the correct season value, and update the frontend queries to filter by season. Existing years' data remains intact and queryable.

---

## Known Limitations & Future Considerations

- **Season partitioning** — all tables have a `season` INTEGER column (e.g. `2026`). App-level filtering by season is groundwork for next year; queries are not yet season-scoped in the frontend
- **Single admin user** — hardcoded, no admin management UI
- **No password reset** — intentional for this use case, but worth addressing if the player pool grows
- **SHA-256 auth** — adequate for a low-stakes friends competition; not suitable for sensitive applications
- **No automated testing** — the codebase is small enough to verify manually, but unit tests for the scoring engine would reduce regression risk year over year

---

*Built with vanilla JS, GitHub Pages, and Supabase. Maintained by EHall4444.*
