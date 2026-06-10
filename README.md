# Lawn Irrigation Tracker

A single self-contained HTML page that takes the guesswork out of watering a
full-sun, south-facing cool-season lawn. It logs when you water, pulls recent
rainfall and evaporation for a fixed location, and tells you **whether to water,
how much, and the best time of day** to do it with minimal evaporation.

No build step, no server, no API key, no accounts. Everything runs in your
browser and your data stays on your machine.

The location is hardcoded to latitude `45.2391`, longitude `-76.1877`. To use a
different spot, edit the `COORDS` constant near the top of the `<script>` in
`index.html`.

## How to run

Option A — just open it:

- Double-click `index.html` (or drag it into your browser).

Option B — serve it locally:

```bash
cd lawn-irrigation
python3 -m http.server 8000
```

Then visit http://localhost:8000.

## What it does

- **Uses a fixed location** (latitude `45.2391`, longitude `-76.1877`) — no
  browser geolocation or permission prompts.
- **Pulls weather** from the free [Open-Meteo](https://open-meteo.com) forecast
  API: the last 7 days plus a short forecast, in mm / °C / km/h.
  - Rainfall: `precipitation_sum`
  - Evaporation: `et0_fao_evapotranspiration` (ET₀, FAO-56 reference
    evapotranspiration for grass — the standard estimate of how much water a
    lawn loses)
- **Tracks your waterings** — log a date and amount in millimetres (with
  quick-add presets). Stored in `localStorage`.
- **Computes a rolling 7-day water balance:**
  `applied = rainfall + your watering` vs. your weekly target.
- **Recommends action:** if you're short of the target, it suggests how much to
  apply (capped at ~13 mm per session to encourage deep, infrequent watering),
  and flags upcoming rain so you don't water needlessly.
- **Recommends timing:** scans tomorrow morning's hourly forecast (≈4–10 AM) and
  picks the lowest-evaporation, low-wind window, because early-morning watering
  minimizes waste and fungal risk.
- **Shows a 7-day breakdown** of rainfall vs. evaporation (and your waterings)
  as simple bars.

## Settings

- **Weekly water target** (default `32 mm`) — cool-season grass (Kentucky
  bluegrass, fine fescue, perennial ryegrass) generally wants 25–38 mm/week,
  rain and watering combined.
- **Clear watering log.**

## Assumptions & notes

- Amounts are tracked in **millimetres**; weather is shown in mm / °C / km/h.
- Tuned for a **cool-season grass mix** (primarily Kentucky bluegrass with fine
  fescue and perennial ryegrass).
- **ET₀** is used as the lawn's water-loss proxy. It assumes well-watered
  reference grass, which is a good general estimate but not a precise model for
  your specific soil, grass type, or slope.
- A south-facing, full-sun lawn dries faster than shaded areas, so the default
  32 mm/week target leans toward the higher end. Adjust it in Settings to suit
  your grass and climate.
- This is **helpful guidance, not a precise agronomic model.** Use common sense
  and local watering restrictions.

## Files

- `index.html` — the entire app (HTML + CSS + JavaScript).
- `README.md` — this file.
