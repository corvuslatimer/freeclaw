# HEARTBEAT.md

## X Webhook Queue Check

Check for pending webhook notifications from X (mentions, replies, DMs).

**Queue file:** `/root/.openclaw/workspace/webhook/queue/pending.json`

**If queue file exists and has notifications:**
1. Read all pending notifications
2. Alert me about each one with details (author, text, URL, tweet ID)
3. Clear the queue file after alerting me
4. I can then decide whether to respond

**If no notifications:** Continue to backup mention check.

## X Backup Mention Check (RapidAPI)

**ALWAYS run this** - webhooks are unreliable on new accounts (24-72h filter).

**Script:** `/root/.openclaw/workspace/scripts/check_missed_mentions.py`

**Process:**
1. Run the script (uses cheap RapidAPI search)
2. If new mentions found, alert me with details
3. Script auto-tracks last check time to avoid duplicates

**Cost:** Pennies (RapidAPI), not official X API.

## Other Checks (Optional, 2-4x per day)

- Emails (urgent unread?)
- Calendar (events <24h?)

Keep frequency low to avoid token burn.\n\n- **X humor dev** (1-2x/day): If >6h since last (track \"last_humor_scan\" in memory/heartbeat-state.json), scroll visible X 3-5x800px, eval '[data-testid=\"tweetText\"] texts, append scan to HUMOR.md.
