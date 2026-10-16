# SkorFire Live Football — showcase

![SkorFire](images/promo-marquee-1400x560.png)

Live scores, today's fixtures, league tables and goal notifications from the browser toolbar. Free, no account, no ads, no tracking. Chrome, Edge and Firefox; 19 languages.

**Install:** [Chrome Web Store](https://chromewebstore.google.com/detail/apallmfeiokillgdbpedodafkgampneh) · [Microsoft Edge](https://microsoftedge.microsoft.com/addons/detail/neljlflmnlamhdkdgdpokgiaieplmmia) · [Website](https://skynonlabs.com/browser-extensions/skorfire/) · [Privacy](https://skynonlabs.com/browser-extensions/skorfire/privacy/) · [Türkçe](README.tr.md)

> This repository is a showcase. The source code is private. Not affiliated with ESPN.

| | |
|---|---|
| ![Match list](images/screenshot-1-en.png) | ![Table](images/screenshot-3-en.png) |
| ![Notifications](images/screenshot-5-en.png) | ![Live stats](images/screenshot-6-en.png) |

## What's new in 2.5

Current version: **2.5.4**

- **Test notification:** a "📣 Test notification" link under each notification switch in Settings sends a sample goal, match-started or reminder notification, so you can check your system settings before a real match.
- **Clickable team names:** a faint dotted underline (solid green on hover) shows that a team name opens the team page.
- **Followed teams** stand out in matte gold, with the star on the outer side of the team.
- **Cleaner notifications:** the image "match started" notification now shows just the match name.

## Architecture

```mermaid
flowchart LR
  P[Popup UI] --> C[(storage cache)]
  P --> E[Single data gateway]
  BG[Service worker + alarms] --> E
  E --> API[Public score JSON]
  BG --> N[Notifications + badge]
```

- **No server, no dependencies, no build step.** Plain HTML, CSS and JavaScript on Manifest V3; running cost is zero.
- **Minimal permissions:** storage, alarms, optional notifications, one API host. No content scripts.
- **Adaptive cache:** 30 s around live matches, 5 min otherwise. A list older than 30 min is never shown on open; an animated logo loading screen waits for fresh data instead.

## Engineering details

- **One gateway for the data source.** Every request goes through one module, so an undocumented API change touches one file.
- **"If unsure, don't show it."** Match state is mapped from the exact status code; postponed, suspended and cancelled games are never shown as full time.
- **Bugs other apps ship, avoided by design:** wrong competition IDs (verified against the live API), old finals shown as today's results (36-hour window), silent 100-match truncation (explicit limit + warning), matches lost at midnight (yesterday, today and tomorrow fetched separately).
- **Offline unit tests** on Node's built-in runner, including locale completeness and version parity checks.
- **19 locales**, every visible string localized; light and dark theme.

© 2026 Skynon Labs Ltd. All rights reserved. · hello@skynonlabs.com
