---
name: reddit-multiaccount
description: How to set up and operate the Reddit multi-account PRAW infrastructure — automated API app creation via old.reddit.com, PRAW operations, and weekly health checks. Load this when setting up accounts or running health checks.
---

# Reddit multi-account — Seren

## Setup (run once at onboarding, then when new accounts are added)

**Human prerequisite (already done by the time you run this):**
Accounts were purchased from REDAccs and stored at `team.reddit_accounts` in vault as a JSON array: `[{"username":"acct_01","password":"delivered_pw"}, ...]`.

**Your job — automated for each account:**

1. Load credentials from `vault_get` at `team.reddit_accounts`.
2. For each account that doesn't yet have a `client_id` + `client_secret` stored:

   a. **Create the API app** via browser automation on `old.reddit.com`: **Create the API app** via browser automation on `old.reddit.com`:
   - Navigate to `https://old.reddit.com/login`, fill `#user_login` and `#passwd_login`, click submit.
   - Navigate to `https://old.reddit.com/prefs/apps/`.
   - Click "create another app...".
   - Fill `#app_name` with `agent-{username[:8]}`, select `script` type, fill redirect URI as `http://localhost`, click "create app".
   - Scrape `client_id` from `.app-details h3` and `client_secret` from `td:text('secret') + td`.

   c. **Store the full config** back to vault. The updated entry format:
   ```json
   {
     "username": "acct_01",
     "password": "new_strong_password",
     "client_id": "scraped_id",
     "client_secret": "scraped_secret"
   }
   ```

3. After all accounts are set up, log the count to `wiki/Knowledge/Reddit/AccountHealth.md` and report success to the co-founder.

## PRAW operations (after setup)

All Reddit interactions after setup use PRAW via `exec` Python, never the browser.

### Posting a comment
```python
import praw, json

accounts = json.loads(vault_get("team.reddit_accounts"))
config = next(a for a in accounts if a["username"] == assigned_username)

reddit = praw.Reddit(
    client_id=config["client_id"],
    client_secret=config["client_secret"],
    username=config["username"],
    password=config["password"],
    user_agent=f"app-{config['username'][:8]}/1.0"
)

submission = reddit.submission(url=post_url)
submission.reply(comment_text)
```

Always use the account assigned by the `reddit-playbook` rotation logic. Never post without co-founder sign-off on the specific draft.

### Timing rules
- 15+ minute gap between any two account actions (enforce via `time.sleep(900+)`).
- 1-2 comments per account per day maximum.
- Vary the exact minute — never post at :00 or :30 exactly.

## Weekly health check (every Monday)

For each account:

1. GET `https://www.reddit.com/user/{username}/about.json`.
   - 404 = account banned or shadowbanned. Flag immediately.
   - Check `comment_karma` and `link_karma` — log both.
2. Check `is_suspended` flag in the response.
3. Post a comment on r/test (or any low-traffic sub) and immediately check whether it appears without login — if it doesn't, the account is shadowbanned.

Log results to `wiki/Knowledge/Reddit/AccountHealth.md` in this format:

```
## Health check YYYY-MM-DD
| Account | Comment karma | Link karma | Status |
|---------|--------------|------------|--------|
| acct_01 | 142 | 12 | ok |
| acct_02 | 87 | 4 | ok |
```

Escalate to the co-founder immediately if any account shows banned, suspended, or shadowbanned status.

## Detection avoidance ⚠️ optional / aggressive

> **Warning — read before enabling.** Operating multiple purchased accounts to post coordinated comments violates Reddit's User Agreement (§ "Don't do anything that interferes with normal use of Reddit" and the ban on coordinated inauthentic behavior). Reddit bans accounts for this and has become significantly better at detecting it since 2023. The posting rules below reduce risk but do not eliminate it. Account bans are likely over time. Enable this only if the co-founder has explicitly accepted that risk.
>
> If you are unsure, skip this section entirely. Seren operates correctly with a single account.

Reddit detects multi-account operations via: IP address, browser fingerprint, cookies, behavioral patterns, rapid creation.

Operating via PRAW (API only) handles fingerprint and cookie leakage automatically — no browser means no viewport, no cookies, no font enumeration. For IP: all Seren's PRAW calls run from the pod's server IP. This is normal and expected for API bots — Reddit explicitly supports this. Do not use a residential proxy unless the co-founder explicitly requests it.

Behavioral patterns are the remaining risk. The posting rules (rotation, timing variance, no account-to-account interaction) reduce exposure but do not guarantee safety.
