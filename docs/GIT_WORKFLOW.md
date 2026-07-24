# Git Workflow

One remote. One truth. Pull before you start, push before you stop.

## 1. Remote

Forgejo only. Never add a second remote for this repo unless you're doing
a deliberate, temporary migration — and remove it the same session.

```bash
git remote -v   # should show exactly one remote: origin -> erichall.dev/...
```

## 2. Branches

- `main` — always deployable, never broken. Don't commit half-finished
  work directly to it.
- `feature/<short-name>` — all in-progress work happens here.
- Merge to `main` only when the feature is actually done.

```bash
git checkout -b feature/notifications
# ...work...
git checkout main
git merge feature/notifications
git branch -d feature/notifications
```

## 3. Commit messages

One logical change per commit. Prefix with the type of change:

| Prefix | Use for |
|---|---|
| `feat:` | new functionality |
| `fix:` | bug fix |
| `docs:` | documentation only |
| `chore:` | maintenance, config, cleanup, dependency bumps |
| `refactor:` | code restructuring, no behavior change |

Example: `feat: add path completion tracking to checkins`

## 4. Every session — no exceptions

**Start of session:**
```bash
git pull origin main
```

**End of session (or before switching machines/tools — Claude Code,
terminal, another Mac):**
```bash
git add -A
git commit -m "..."
git push origin main
```

Uncommitted or unpushed work sitting on one machine while you work
from another is how repos fork. This one habit prevents it.

## 5. Second clones

If you ever need a second local copy of a repo (testing, experimenting),
either delete it when you're done or explicitly sync it back to `main`
within the same session. Never let a second clone sit untouched for
weeks — it will accumulate its own history and fork silently.

## 6. `.gitignore` basics

Every repo should ignore at minimum:
```
.DS_Store
.env
.env.*
node_modules/
```