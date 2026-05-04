# Quickstart

## What this is

A scheduled Claude agent runs twice daily, pulls news on the topics in `topics.yaml`,
filters by `preferences.md`, and regenerates `feed.xml` — served by GitHub Pages and
read in Inoreader on Android.

```
   Anthropic-hosted scheduled agent (6:33am + 2:33pm ET)
           │
           │  clone repo → read topics + prefs + log
           │  fetch RSS + web search → dedupe → score
           │  rewrite feed.xml + log → commit + push
           ▼
   GitHub Pages ──► https://mhsezgin.github.io/news-feed/feed.xml
                                   │
                                   ▼
                        Inoreader on Android
```

## Files (and who owns them)

| File | Owner | Purpose |
|---|---|---|
| `topics.yaml` | **You** | RSS sources + topic keywords. Edit to add/remove sources. |
| `preferences.md` | **You** | Natural-language prefs. Always-include / down-weight / tone. |
| `feed.xml` | Agent | Don't edit by hand. Agent regenerates each run. |
| `published-log.json` | Agent | Dedupe log, last 500 items. Don't edit. |
| `digest-archive/` | Agent | Per-run markdown copies for review. |
| `meta-instructions.md` | You/Claude | How Claude sessions should update prefs. |
| `README.md` / `quickstart.md` | You | Human docs. |

## Updating preferences

Either:
- **Edit `preferences.md` directly**, then `git add -A && git commit -m "prefs: ..." && git push`. Next run picks it up.
- **Tell Claude in this directory**: *"less crypto, more EU AI regulation"* — it'll edit + push for you.

To add a new topic area, also add it to `topics.yaml` with at least 2 RSS sources and
a few keywords. Verify URLs return 200 before committing:
```sh
curl -s -o /dev/null -w "%{http_code}\n" https://example.com/feed.xml
```

## Routine management

The scheduled agent is a "routine" in Anthropic's cloud. Routine ID:
`trig_01R8uDvpjkCYgAfwq6ZaeKpC`.

- **UI**: https://claude.ai/code/routines/trig_01R8uDvpjkCYgAfwq6ZaeKpC
  → toggle enabled/disabled, edit cron, view run history + token usage.
- **Tell Claude in this directory** to:
  - "Run the news routine now" → manual fire.
  - "Change cadence to ..." → updates the cron.
  - "Update the routine prompt to ..." → updates the agent's instructions.

Current cron: `33 10,18 * * *` UTC.
Local fires (EDT, UTC−4): **6:33am** and **2:33pm**.
**DST caveat**: In November when EST returns, fires will shift to 5:33am and 1:33pm
local. Tell Claude to "re-pin the cron for EST" when that matters.

## Common shell commands

```sh
# View latest digest commit
git -C "/Users/erkansezgin/Python Projects/personal claude/news-feed" log --oneline -5

# Pull latest digests onto the laptop
cd "/Users/erkansezgin/Python Projects/personal claude/news-feed" && git pull

# Validate the live feed served by Pages
curl -s https://mhsezgin.github.io/news-feed/feed.xml | xmllint --noout - && echo OK

# Force-deploy after a manual feed.xml edit (Pages auto-deploys on push)
git push  # that's it; the workflow in .github/workflows/pages.yml handles the rest

# Check Pages workflow status
gh run list --repo mhsezgin/news-feed --limit 3
```

## Subscribe URL (for any RSS reader)

```
https://mhsezgin.github.io/news-feed/feed.xml
```

## Troubleshooting

- **Inoreader still shows old feed title or stale entries.** RSS readers cache
  feed-level metadata. To refresh: long-press subscription → *Rename* (cosmetic), or
  unsubscribe and resubscribe (forces full refresh, loses local read/star history).
- **Run failed / no new commit at the expected time.** Check the routine UI run log.
  Common causes: a transient network blip on an RSS source (the agent continues with
  others); GitHub push rejected (stale clone — the agent retries with `pull --rebase`).
- **PAT expired.** The fine-grained PAT for `news-feed` has a 1-year expiry. When it
  expires, regenerate at https://github.com/settings/personal-access-tokens, then ask
  Claude to "update the routine with new PAT `<token>`" — it will swap the token in
  the routine's prompt.
- **Repo got noisy with empty commits.** When all topics yield zero new items, the
  agent commits a timestamp-only update. Set `topics.yaml` keywords broader, or
  reduce cadence.
- **Want to pause without deleting.** Toggle "Enabled" off in the routine UI, or
  ask Claude to "disable the news routine."

## Cost reminder

Each run uses ~200–250K tokens (input-heavy: RSS payloads + article bodies fetched
for richer summaries). At 2 runs/day, that's ~12–15M tokens/month. Check actual
usage at https://claude.ai/code/routines/trig_01R8uDvpjkCYgAfwq6ZaeKpC or the
account usage page.

## Calendar these

- **April 2027** — regenerate the GitHub PAT before it expires (1-year expiry from
  creation in May 2026). Old token stops working silently; new digests just stop
  pushing.
