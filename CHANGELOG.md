# Changelog

All notable changes to the Siuslaw Tide Tracker. Live site: https://nwfella.github.io/florence-tide-tables/

## v1.2.0 — 19 Nov 2026

**Default-tab setting, self-refresh, and the roadside camera.**

- **Choose the tab that opens first.** New “Remember this tab” checkbox in the header stores the visitor's choice in `localStorage` (`defaultStation`). Precedence is **URL → remembered → Florence**, and the choice is read before the first render, so a pinned tab never flashes the other one (or pays for its fetch). Added `?station=florence` / `?station=cushman` for shareable links; a link sets the view for that visit without overwriting the recipient's saved default. Unrecognised values are ignored.
- **ODOT camera** in the Cushman panel — the TripCheck still from **OR 126 at Cushman, MP 3.05, looking east** at the trestle underpass, re-fetched every 60 s while visible with a cache-buster, and a graceful fallback to a TripCheck link if ODOT re-publishes the file.
- **Self-refresh** — tide/river/alerts every 10 minutes while the tab is visible, plus on tab focus when the last render is over 60 s old. Nothing polls while the tab is hidden. A `· updated 3:14 PM` stamp in the header makes a stale tab obvious. No server, no cron, no API key, no LLM.
- **Cheaper polls** — tomorrow's NOAA predictions and the USGS p50 median are memoised for the session, so a refresh cycle is 4 requests (~13.5 KB) instead of 6. Failures are never cached.
- **Fixed:** “Next High” went blank once today's high had passed; the card now fills the missing side from tomorrow and labels it (`3:24 AM · tomorrow`) instead of showing `—` until midnight.
- Version marker in the footer linking to releases.

## v1.1.0 — 27 Oct 2026

**Cushman and the OR 126 road watch.**

- Second station: **Cushman, Siuslaw River (NOAA 9434068)**, with a header switcher and one shared date picker for both views.
- **OR 126 high-water panel** for Cushman: verdict (clear / borderline / overflow), the day's predicted peak, the window the tide sits at or above a flooding mark (adjustable 7.5–9.0 ft, persisted), live USGS discharge and gage height at Mapleton compared with that calendar day's median, and active NWS flood/coastal alerts.
- Dashed flooding-mark line on the chart, and a documented heuristic rule binding tide + river + alerts, with sources for the ~8 ft anchor and explicit limits (predictions only — the station's sensor was removed in 2018; ODOT decides closures).
- **Fixed:** the page silently used a synthetic curve while claiming “NOAA Live” — `date=exact` is not a valid NOAA parameter and every request was returning HTTP 400. Now `begin_date=YYYYMMDD&range=24`.
- **Fixed:** local-date handling (was using UTC `toISOString()`, which rolled over to “tomorrow” every afternoon in Pacific), next-event filtering, and a `--` countdown that never updated.

## v1.0.0 — 27 Oct 2026

- Initial single-file NOAA tide dashboard for **Florence, Oregon** (station 9434098): current level interpolated from six-minute predictions, next high/low with countdown, 24-hour tide curve with a `NOW` marker, daily extremes table, arbitrary date selection, datum meter, and an honestly-labelled offline fallback.
- Deployed to GitHub Pages at https://nwfella.github.io/florence-tide-tables/
