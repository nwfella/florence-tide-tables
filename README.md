# Florence Tide Tables

A single-file tide dashboard for **Florence, Oregon** — Siuslaw River / Florence USCG Pier, NOAA station **9434098** — built on the official [NOAA CO-OPS Data API](https://api.tidesandcurrents.noaa.gov/api/prod/).

**Live:** https://nwfella.github.io/florence-tide-tables/

![Screenshot of the Florence Tide Tables dashboard](docs/preview.png)

## What it does

- **Current water level** for the station, interpolated from six-minute NOAA predictions (not snapped to the nearest sample), with a rising/falling trend badge and a datum meter on a −2 → +9 ft scale.
- **Next high / next low** — the next events *still ahead of you*, with a live countdown. After the last tide of the day it pulls tomorrow's first events instead of showing a time that already passed.
- **24-hour tide curve** from six-minute predictions, with a dashed `NOW` marker at your current position.
- **Full daily extremes table** with passed / upcoming status.
- **Any date**, past or up to a year ahead, via the date picker.

## Data notes

| | |
| --- | --- |
| Product | `predictions` (harmonic tide predictions, **not** observations) |
| Station | 9434098 — Florence USCG Pier, Siuslaw River, OR (44.0021° N, 124.1230° W) |
| Datum | MLLW, feet |
| Time zone | `lst_ldt` — station local time, DST-aware |
| Interval | `hilo` for extremes, default six-minute for the curve |

Tide predictions are astronomical, not measured water levels: actual water can sit well off the curve when wind, river flow or barometric pressure push it. **Never use this for navigation.**

There is an offline fallback — a small mixed-harmonic approximation tuned to Florence's ~10 ft range — that only appears when the API request fails. It is labelled *Offline Estimate* in the UI and is not NOAA data.

## Build

No build step, no framework, no bundler. One `index.html`:

```bash
git clone https://github.com/nwfella/florence-tide-tables.git
cd florence-tide-tables
# open index.html directly, or:
python -m http.server 8000
```

Runtime dependencies are three public CDNs: Tailwind (Play CDN), Chart.js 4, and Lucide icons. Fonts come from Google Fonts. The NOAA API allows cross-origin reads (`Access-Control-Allow-Origin: *`), so no proxy or key is required.

### Gotcha worth recording

The API rejects `date=exact`. An earlier revision passed `date=exact&begin_date=YYYYMMDD`, which returns **HTTP 400 `Wrong Date: The supported date values are: latest, recent, or today`** — every request failed and the page silently fell back to the synthetic curve while still claiming "NOAA Live". The correct form is `begin_date=YYYYMMDD&range=24` (or an explicit `end_date`). If you fork this, keep an eye on the data-source badge: it is green only when NOAA actually answered.

## License

MIT — see [LICENSE](LICENSE).
