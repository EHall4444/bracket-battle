# Bracket Battle — Manual Password Reset

**When to use:** User can't log in and can't remember their password.

## Step 1 — Verify identity
Ask the user something only they'd know (username, recent game picks,
registration date, etc.) before making any changes.

## Step 2 — Open Supabase SQL Editor
Go to your Supabase dashboard → SQL Editor.

## Step 3 — Confirm the username exists
```sql
SELECT username, password_hash FROM users WHERE username = 'their_username';
```
Make sure you have the right account before changing anything.

## Step 4 — Choose a temp password
Pick something simple to communicate, e.g. `Lacrosse1`.

## Step 5 — Generate a SHA-256 hash
Go to: https://emn178.github.io/online-tools/sha256.html
Type the temp password exactly as-is. Copy the 64-character hash.

> **Note:** this sends the temp password to a third-party site. Worth
> replacing with a local hash generation method eventually — see below.

## Step 6 — Update the database
```sql
UPDATE users
SET password_hash = 'paste_hash_here'
WHERE username = 'their_username';
```
Double-check the username before running.

## Step 7 — Confirm it worked
```sql
SELECT username, password_hash FROM users WHERE username = 'their_username';
```
The hash should now match what you generated in Step 5.

## Step 8 — Tell the user
Give them the temp password and ask them to change it after logging in.

---

## Known issues to fix eventually
- Step 5 relies on an external website to hash the password — replace
  with a local one-liner instead, e.g.:
  ```bash
  echo -n 'Lacrosse1' | shasum -a 256
  ```
  Run entirely on your own machine, nothing sent anywhere.
- Plain unsalted SHA-256 is weak for password storage generally. Low
  priority for a once-a-year hobby app, but worth revisiting if this
  app ever handles anything more sensitive.
