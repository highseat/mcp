---
name: explain-traffic-change
description: Find where a sudden rise or drop in a website's traffic came from in Highseat. Use when asked why traffic spiked, fell, or looks unusual on a site.
---

# Explain a traffic change

1. Call `getStats` with `report: "timeseries"`, `granularity: "day"` and a period that covers the change and the weeks before it, such as `period: "30d"`. Find the first day that breaks from the usual level.
2. For that day, call `getStats` with `period: "custom"` and `from` and `to` both set to the day. Run `report: "sources"`, `report: "pages"` and `report: "locations"`, then the same three for a normal day the week before.
3. Compare the two days row by row. The cause is usually one source, page or country whose count moved far more than the rest. Name it with both counts.
4. When the change is a campaign, `report: "campaigns"` with `dimension: "utm_campaign"` names it.
5. Call `listNotes` with `from` and `to` around the day. A note may name a launch or a newsletter sent that day.

Check two causes that are not visitors:

- A sudden drop to zero usually means the tracker stopped loading. Call `getInstallStatus`: an old `lastSeen` or a `rejected` host confirms it.
- A spike that `report: "bots"` also shows is bot traffic, which Highseat leaves out of the stats.

Before filtering on a value, read it with `getFilterValues`. Say which figures support the explanation, and say so when the data does not show a cause.
