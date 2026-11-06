# Airport Weather 2025: methods and QC

File: `airport_weather_2025.csv`, one row per airport per day.
327 airports x 365 days = 119,355 rows. Join to flight data on `airport_code` + `date`.

## Source

NOAA GHCN-Daily (Global Historical Climatology Network, Daily), 2025 by-year file,
downloaded from ncei.noaa.gov. This is the dataset behind the NCEI Past Weather
viewer at ncei.noaa.gov/access/past-weather; the viewer itself has no bulk export,
so the pull was done from the underlying dataset files. Accessed Nov 5, 2026.

## Station selection (one station per airport)

From the GHCN station inventory, filtered to stations reporting in 2025:
nearest station to the airport reporting TMAX, TMIN and PRCP, preferring a
station that also reports SNOW and SNWD when one is within 10 km.
Result: median station distance 0.85 km (usually the airport's own ASOS),
302 of 327 within 5 km, worst case Hilton Head at 28.8 km (Beaufort MCAS).
Full mapping with distances: `station_mapping.csv`.

## Columns and definitions

- `avg_temp_f`: DERIVED as (max + min) / 2. Reported TAVG exists at only 33 of
  the 327 stations in GHCN-Daily, so a reported-only column would be mostly blank.
- `max_temp_f`, `min_temp_f`: station TMAX / TMIN, converted from tenths degC.
- `precip_in`: PRCP, tenths of mm converted to inches. `precip_trace` = Y means
  a trace amount was measured (value recorded as 0.00).
- `snowfall_in`, `snow_depth_in`: SNOW / SNWD, mm converted to inches.
  `snow_trace` works like `precip_trace`.
- Blank means NOT measured or not reported. Nothing is zero-filled: stations in
  Hawaii, coastal California, the territories and some others do not report
  snowfall or snow depth at all, and stations also have ordinary reporting gaps.

## QC performed

- Exactly 119,355 rows and 119,355 unique (airport_code, date) keys, 327 airports.
- min <= avg <= max holds on every row; zero rows with min > max; no negative precipitation.
- 659 values carrying a GHCN quality flag were treated as missing rather than passed through.
- Derived avg temp cross-checked against reported TAVG on the 5,502 station-days
  that have both: mean absolute difference 1.45 F (reported TAVG is an hourly mean,
  the derived value is the max/min midpoint, so a small systematic gap is expected).
- Spot checks against known 2025 events: Jan 21, 2025 Gulf Coast blizzard shows
  7.99 in at New Orleans (MSY), 7.48 in at Mobile (MOB), 8.90 in at Pensacola (PNS),
  matching the reported storm records. Hottest value in file: 118.9 F, Palm Springs,
  Aug 7. Coldest: -47.9 F, Fairbanks, Dec 27.
- Coverage (rows with a value, of 119,355): max temp 117,421; min temp 117,344;
  avg temp 117,245; precipitation 114,654; snowfall 84,095; snow depth 79,355.
  Note: Boston Logan's GHCN station reports snowfall but not snow depth, so
  BOS snow_depth_in is blank throughout; that is a property of the source station.

Rebuild: `build_weather_csv.py` in this folder; raw inputs in `raw/`
(2025.csv.gz, ghcnd-stations.txt, ghcnd-inventory.txt); numbers in `qc_report.json`.
