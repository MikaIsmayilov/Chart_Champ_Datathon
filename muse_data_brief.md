# Research brief: external data for flight-connection assumptions

## Background
We are a student team in a university datathon, acting as consultants to a US company whose employees fly often. Our goal is to recommend ways to reduce travel disruption (delays and cancellations).

Our main dataset is the 2025 BTS Reporting Carrier On-Time Performance data: US domestic flights for ten carriers (AA, AS, B6, DL, MQ, OH, OO, UA, WN, YX). It records individual flight legs only. It has no passenger itineraries, so we cannot see connections, fares, or costs.

We need outside sources to support assumptions about connections and the cost of delay. Every figure will be cited in a presentation to judges, so each one must be traceable to a source.

## What to find

### 1. Minimum connection times (highest priority)
- **What:** the published minimum connection time for domestic-to-domestic connections, by airline and airport.
- **Airlines:** American, Delta, United, Alaska, JetBlue, Southwest.
- **Airports:** each airline's main US hubs, for example ATL, DFW, ORD, DEN, CLT, IAH, EWR, MSP, DTW, SLC, SEA, PHX, MIA, PHL, DCA, LAX, SFO, JFK, BOS. If an airline publishes one system-wide figure, report that.
- **Where to look:** the airlines' own websites and contracts of carriage first. The OAG/IATA minimum connection time database is paid; note it if it is the only source, but do not rely on it.
- **Also useful:** any source for the typical or average scheduled layover on US domestic connecting itineraries.

### 2. How many travelers connect
- **What:** the share of US domestic passengers on connecting itineraries versus nonstop, overall and by hub airport if available.
- **Where to look:** the BTS Airline Origin and Destination Survey (DB1B), or published summaries based on it. The most recent year available is best.
- **Also useful:** the download location and file format for the DB1B data, in case we want to compute this ourselves.

### 3. What delays and missed connections cost passengers
- **What:**
  - The value of travel time per hour for business air travelers, from US DOT guidance on the valuation of travel time.
  - Estimates of passenger delay caused by missed connections and cancellations, compared with delay from late flights alone.
- **Where to look:**
  - US DOT, "Revised Departmental Guidance on Valuation of Travel Time in Economic Analysis" (find the latest revision).
  - NEXTOR, "Total Delay Impact Study" (2010).
  - Barnhart, Fearing and Vaze, MIT, on passenger-centric delay in the US air transportation system.
  - Any more recent study that updates these figures.

### 4. Rebooking after a missed connection (lower priority)
- **What:** any published figure for how long passengers typically wait to be rebooked after a missed connection or cancellation on US domestic flights.

## What to return
One row per figure, in a table with these columns:

| Column | Content |
|---|---|
| Item | Which numbered item above it answers |
| Figure | The exact number and unit |
| Applies to | Airline, airport, passenger group, and year |
| Source | Title and publisher |
| URL | Direct link to the page or document |
| Published | Date of the source |
| Accessed | Date you retrieved it |
| Quote | The sentence or table cell the figure comes from |
| Notes | Caveats, for example "system-wide figure" or "paywalled" |

## Rules
- Report only figures you found in a source. If you cannot find one, write "not found" and say where you looked.
- Prefer primary sources (airlines, BTS, DOT, the original study) over news articles and blogs. If you use a secondary source, say so.
- Keep figures from different years separate; do not average or combine them.
- Flag anything older than 2020 as possibly out of date.
- Flag any place where two sources disagree, and give both.
