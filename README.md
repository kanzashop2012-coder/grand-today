# Grand Today — Front Office Daily Operations Sheet

A single-file, offline-first web app for building a resort Front Office's
daily arrival & departure operations brief: import Opera PMS exports
(Arrivals, Departures, Reservation Summary, Guest In-House Meal Plan),
auto-fill weather/tide/sun data, auto-sequence airport-transfer boat runs
from the flight schedule, and produce the day's Excel sheet, departure
letters, welcome cards and amenity request list — all from one click.

No build step, no server required. It's one HTML file (`index.html`) that
runs entirely in the browser; data is kept in `localStorage` unless you
wire up the optional live-sync described below.

## Features

- **Import**: Opera PDF/XLSX exports for arrivals, departures, and
  occupancy/meal-plan/nationality reports, or paste rows copied from Excel.
- **Auto-fill**: sunrise/sunset/tides/3-day weather (via Open-Meteo),
  live flight times, nationality & meal-plan tallies.
- **Boat allocation**: groups arrivals by flight + ETA, applies a
  configurable per-boat passenger cap and a rolling time window, and
  sequences the runs automatically.
- **Produce**: the day's Excel sheet (appended to a running month
  workbook), print-ready departure letters, Word welcome cards, and an
  arrival amenity request sheet.
- **Optional live sync**: multiple stations (desk, supervisor, duty
  manager) can share one sheet in real time via Firebase Firestore, with
  simple PIN-based roles. Fully optional — the app works standalone
  without it.

## Setup

This repo ships as a generic template with all property-specific details
stripped out. Before using it for your own property, edit these constants
near the top of `index.html`:

| What | Where | Notes |
|---|---|---|
| Property name / masthead | `<span class="brand">…</span>` and the `.l1/.l2/.l3` divs in `<body>` | Static HTML text |
| Villa/room count | `VILLA_COUNT` | |
| Coordinates for weather/tide | `PROPERTY_LAT`, `PROPERTY_LON`, `PROPERTY_TZ` | Defaults to Malé, Maldives |
| Logo (letters & welcome cards) | `PROPERTY_LOGO` | Base64 data-URI; placeholder is a blank pixel |
| Front Office roster | `DEFAULT_ALLOC`, `duty` | Example data only — replace with your own |
| Default letter signer | `sign` object in `openDepLetters()` | |
| Welcome card text | `WELCOME_DEFAULT` (also editable from the UI) | |
| Live sync (optional) | `FIREBASE_CONFIG` | Create your own free Firebase project (Firestore + Anonymous Auth) |
| PIN roles (optional, needed only with live sync) | `PIN_HASH` | Each entry is `sha256(pin) → role`. Generate your own in the browser console: `await sha256('yourPIN')` |

Nothing will leak between properties: without your own `FIREBASE_CONFIG`
and `PIN_HASH` values, the live-sync button simply stays in "offline" mode
and the app works entirely local to the browser.

## Usage

Just open `index.html` in a browser — no install, no dependencies beyond
what's already loaded from CDN (Firebase SDK, `xlsx`, `pdf.js`).

## Data sources this expects

Built against Oracle Opera PMS report exports (Arrivals: Detailed,
Expected Departure, Reservation Summary, Guest In-House Meal Plan) and,
for the Maldives, the FIS flight-information feed at fis.com.mv. Column
auto-detection is regex-based and can be adjusted in `HEAD_MATCH` if your
property's report layout differs.

## License

MIT — see [LICENSE](LICENSE).
