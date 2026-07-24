# CLAUDE.md

## Project
Bracket Battle is a single-page web app (one `index.html`, no build step) where a small group picks winners for the NCAA Division I Men's Lacrosse Championship bracket — 16 teams, 15 picks, one champion — and tracks scores on a shared leaderboard. It's deployed as static assets via Cloudflare Workers (`wrangler.jsonc`) with Supabase as the backend for users/picks.

## Where things live
- `docs/GIT_WORKFLOW.md` — exists
- `docs/PASSWORD_RESET.md` — exists (manual admin runbook for resetting a user's password via Supabase SQL)
- `docs/ARCHITECTURE.md` — **missing**, not yet written
- `docs/DECISIONS.md` — **missing**, not yet written
- `docs/DEPLOY.md` — **missing** — deployment steps currently live at root in `STEP-BY-STEP-DEPLOYMENT.md` instead; consider moving/renaming into `docs/DEPLOY.md` to match convention
- `CHANGELOG.md` — **missing**, not yet started

## Git rules
- One remote only: Forgejo. Never add a second remote without removing it in the same session.
- `git pull origin main` at the start of every session.
- `git push origin main` before ending a session or switching tools/machines.
- Commit prefixes: `feat:` `fix:` `docs:` `chore:` `refactor:`
- Never commit `.env` or secrets.

## Working style
- Confirm before force-pushing or rewriting git history, given this repo's past merge history.
- Before diving into a topic, provide a mental model and ask where I want to drill down.
- Use analogies when explaining complex technical work.
- I am a visual learner — one picture can save us 10 exchanges.
- I typically brainstorm and ideate in Claude Desktop and/or Mistral.
- I structure my builds using /speckit in Claude Code.

## Known gotchas
- **`calcScore()` deliberately has no penalty for wrong picks.** An earlier version (still visible in old commit history) subtracted points for incorrect picks — including pre-penalizing a pick as soon as it was mathematically eliminated, before the round even finished. That penalty logic was intentionally removed. The FR-round loop even has an explicit comment marking this: `// FR: one pick per game, correct picks only — no penalty for wrong picks`. The `LOSS` constant is still declared in the function but is unused — that's expected, not a bug. Don't reintroduce score subtraction on a wrong pick without confirming it's actually wanted; it changes the scoring model app-wide, not just cosmetically.
- **Leaderboard tiebreakers**, in order: total score → most upsets picked correctly → `boldScore` (sum of winning seed numbers, higher = bolder) → username alphabetical. This is unconflicted/stable logic — don't confuse it with the scoring-penalty question above, they're separate.
- **Passwords are hashed with plain, unsalted SHA-256** (`h256()` in `index.html`, via `crypto.subtle.digest`). See `docs/PASSWORD_RESET.md` for the manual reset runbook and its own "known issues" note. This is a known, accepted limitation for a once-a-year hobby app — not something to silently "harden" (e.g. adding salting, bcrypt, etc.) without asking first, since that would require a migration path for existing stored hashes.
