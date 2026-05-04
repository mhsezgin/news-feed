# news-feed

Personal news digest. A scheduled Claude agent runs twice daily, pulls items on the topics in
`topics.yaml`, filters by `preferences.md`, and regenerates `feed.xml` — which is served by
GitHub Pages and read in Inoreader on Android.

**Subscribe URL:** `https://mhsezgin.github.io/news-feed/feed.xml`

## Files

- `topics.yaml` — Source RSS feeds + topic keywords. Curated by hand.
- `preferences.md` — Natural-language preferences. Edit directly or via Claude Code.
- `feed.xml` — Owned by the scheduled agent. Don't edit by hand.
- `published-log.json` — Dedupe log. Owned by the scheduled agent.
- `digest-archive/` — Optional dated markdown copies of each run.
- `meta-instructions.md` — How Claude sessions should update preferences.

## Updating preferences

Either edit `preferences.md` directly, or open Claude Code in this directory and say something like:

> "Less crypto, more EU AI regulation. Also add a section for Turkish constitutional law."

Claude will update `preferences.md` (and `topics.yaml` if a new area is added) and push. The next
scheduled run picks it up.
