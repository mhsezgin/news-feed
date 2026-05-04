# Instructions for Claude sessions editing this repo

If the user asks you to update news preferences (e.g. "less crypto", "more EU AI regulation",
"add a Brazilian politics section"), do this:

1. Read `preferences.md`. Edit the relevant section ("Always include", "Down-weight or skip", or "Tone").
2. Keep entries terse — one bullet per preference, no explanation paragraphs. The scheduled agent
   reads this file in full each run; brevity matters.
3. If the user wants a whole new topic area, also add it to `topics.yaml` with at least 2 RSS sources
   and 3-5 keywords. Verify each RSS URL with `curl -s -o /dev/null -w "%{http_code}" <url>`
   before committing — dead feeds break the agent.
4. Commit with message `prefs: <one-line summary>` and push.
5. Tell the user what you changed and that the next scheduled run (next 8am or 7pm local) will pick it up.

Do NOT touch `feed.xml` or `published-log.json` manually — those are owned by the scheduled agent.
