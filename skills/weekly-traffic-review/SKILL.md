---
name: weekly-traffic-review
description: Review a website's last seven days in Highseat against the week before. Use when asked for a weekly traffic review, a weekly report, or how a site did this week.
---

# Weekly traffic review

Use the Highseat MCP tools. Each step is one `getStats` call unless it names another tool.

1. Call `listSites`. If the person did not name a site and more than one is listed, ask which one. To review every site at once, call `listSiteOverviews` instead and stop after step 2.
2. Call `getStats` with `report: "overview"`, `period: "7d"` and `compare: "previous"`. Note visitors, pageviews, bounce rate, session duration and revenue, each with its change.
3. Call `getStats` with `report: "timeseries"`, `period: "7d"` and `granularity: "day"` to find the best and the worst day.
4. Call `getStats` with `report: "pages"` and `report: "sources"`, both with `period: "7d"` and `limit: 10`.
5. Call `getStats` with `report: "goals"` and `period: "7d"`. Skip it when the site has no goals.
6. Call `listNotes` with `from` and `to` set to the seven days the review covers. A note marks an event such as a launch or a campaign.

Write the review in this order:

- One sentence with the week's visitors and the change against the week before.
- The figures from step 2 as a short table.
- The top five pages and the top five sources, with any new entry or large mover named.
- Goal conversions, when the site has goals.
- Each note from step 6 beside the day it explains.

Read the numbers as the server describes them. Money is in minor units of `meta.currency`, so 14307 is 143.07. A source with an empty `name` is direct traffic. State the date range from `meta.range`, in the site's timezone. Do not guess a cause the data does not show.
