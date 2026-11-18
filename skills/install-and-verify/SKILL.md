---
name: install-and-verify
description: Install the Highseat tracker on a website and confirm that events arrive, using getInstallStatus. Use when asked to add Highseat to a site, or to check whether Highseat is installed and working.
---

# Install the tracker and check it works

## 1. Find the site

Call `listSites`. Match the site by its domain. Its `id` starts with `site_`, and the snippet needs it.

If the site is missing and the connection can make changes, call `saveSite` with `name` and `domain` to create it, then call `listSites` again. If the connection only reads, ask the person to add the site in the Highseat dashboard.

## 2. Install

When you are working in the site's code, fetch https://highseat.com/skill.md and follow it with the site's `id`. It lists the steps for Next.js, React, TanStack Start, Astro, WordPress and plain HTML. When you are not in the site's code, point the person to Installation settings in the Highseat dashboard, which shows the snippet for each platform.

Never ask the person to paste a secret into the chat.

## 3. Check that events arrive

After the change is deployed and someone has opened a page on the live site, call `getInstallStatus` with the site. Read the answer in this order:

| Field | What it means | What to do |
| --- | --- | --- |
| `installed` false, all else empty | No event has arrived yet | Check the snippet is on the deployed page, open the page, wait a minute and call again |
| `rejected` set | Events came from a host the site does not count | If `isParent` is true, the site's domain is too narrow (`www.example.com` under `example.com`): widen it. If `allowable` is true, add the host to `extraHostnames` with `saveSite` |
| `local` set | Events came from localhost | Expected during local development. These events are never counted |
| `proxyHits` above 0 | A proxy sent events without the site's secret | Set the secret on the host, as the install steps describe |
| `filtered` above 0 | Events were left out as bots, spam or excluded IPs | Usually normal. Call `getStats` with `report: "bots"` to see the rules |
| `installed` true | Events arrive. `firstEventAt` and `lastSeen` say when | Tell the person it works |

Call `getInstallStatus` again after each fix. Stop after three checks without a first event, and tell the person what you found.
