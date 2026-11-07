# Chart Champ Datathon: Cutting Flight Disruption

A data-driven travel policy proposal for a national corporate travel program, built for the
Questrom Chart Champ Datathon (BA891), final on November 7, 2026.

**Team 2:** Mika Ismayilli, Steven Marathias, Burak Ataseven

## Final presentation (November 7, 2026)

- **Slides (PDF, 11 pages):** [Chart_Champ_Slides_Team2.pdf](Chart_Champ_Slides_Team2.pdf)
- **Demo video, Safe Flight (2 min 42 s):** [Safe_Flight_Demo_Video.mov](Safe_Flight_Demo_Video.mov)

## The brief

Act as consultants responding to an RFP from a national company whose employees fly
frequently: recommend 2 to 4 travel-policy or booking changes that reduce delay and
cancellation disruption, and support every recommendation with evidence.

## Headline findings

- **9.74% of flights are major disruptions**: cancelled, diverted, or arriving 60 or more
  minutes late (200,000-flight sample of 2025 US flights).
- **Disruption rises through the day.** By scheduled departure hour, the disruption rate
  climbs from 4.7% at 06:00 to a 16.1% peak at 19:00.
- **The early-flight effect survives matching.** Comparing flights on the same route, with
  the same carrier, in the same month (full 2025 population of 6,398,056 flights):
  early departures (06:00-08:59) disrupt at **4.94%**, late departures (15:00-20:59) at
  **14.48%**. A gap of 9.5 percentage points that is not explained by which routes or
  airlines fly late.
- **It compounds.** The worst month x departure-window cell is July, 18:00-20:00, at
  27.5%. The safest is October, 00:00-05:00, at 2.0%.
- **The mechanism is a cascade, not weather.** Of arrival-delay minutes on flights with
  BTS cause data (n = 43,512): late aircraft 39.0%, carrier 32.8%, National Airspace
  System 21.6%, weather 6.5%, security 0.1%. Most delay is inherited from earlier flights
  in the day, which is exactly what an early departure avoids.
- **Snow is a separate regime.** Disruption by origin-airport snowfall that day: 9.6%
  with no snow, 20.1% under 1 inch, 34.6% at 1 to 3 inches, 45.1% at 3 or more inches.
  Booking policy cannot use the day-of forecast, so we use each airport's historical
  snow-day frequency for that month (climatology) instead.
- **The tail is the problem.** Share of flights failing to arrive within X minutes of
  schedule (cancelled and diverted counted as never arriving): 30 min, 15.8% (about
  1 in 6); 60 min, 9.6% (1 in 10); 90 min, 6.7% (1 in 15); 180 min, 3.3% (1 in 30).
  The median flight arrives 6 minutes early, which is why averages hide this.
- **Price does not buy reliability.** Positioning the 10 dataset carriers on average
  itinerary fare (BTS DB1B, 2025 H1) vs disruption rate: some carriers charge premium
  fares for above-average disruption, while the cheapest large carrier is also one of
  the most reliable. Overall average fare $347, overall disruption 9.74%.

## Recommendations

1. **Book the early feasible option** on supported routes: default booking guidance to
   departures before 09:00 where the itinerary allows it.
2. **Tiered arrival buffers, and prior-day travel for critical meetings**, sized from the
   tail numbers above rather than from average delay.
3. **Score every booking at purchase time.** The companion demo is a booking site where
   each flight option carries an individual risk score from a supervised model, so the
   policy is enforced at the moment of choice, not in a PDF nobody reads.
4. **Pilot before scaling:** one travel segment for one quarter, booked-flight disruption
   measured against a matched baseline, fare premium per trip tracked.

Break-even, stated as a scenario with explicit assumptions (not measured savings): at an
assumed $500 loss per disrupted trip and $10 other cost, the measured 9.5-point risk gap
supports a fare premium of roughly $42 per trip for the early option.

## Data and methods

- **Flights:** BTS On-Time Performance 2025. Analysis sample: 200,000 flights, uniform
  random reservoir sample (seed 20261103) from the 6,398,056-flight population;
  sample weight 31.99028. Matching and headline rates were verified on the full
  population files.
- **Outcome:** major disruption = cancelled, diverted, or arrival delay of 60+ minutes.
  One definition is used throughout the deck.
- **Matching:** strata of route x carrier x month, minimum 20 flights per departure
  window; covers 63.4% of flights in the two compared windows.
- **Weather:** NOAA GHCN-Daily 2025, one nearest station per airport (median distance
  0.85 km), 327 airports x 365 days = 119,355 airport-days. Average temperature derived
  as (max + min) / 2. Missing values mean not measured and are never zero-filled.
  Observed day-of weather is never used as a booking-time feature (that would be
  leakage); only airport x month climatology is.
- **Fares:** BTS DB1B Ticket survey, 2025 Q1 and Q2 (9,632,721 sampled itineraries).
  Passenger-weighted mean itinerary fare by reporting carrier. These are not the BTS
  press-release average-fare series, which uses a different one-way-equivalent basis.
- **Model:** supervised gradient boosting / random forest comparison under a predeclared
  protocol (train Jan-Aug, select on September Brier score, Q4 held out as a
  retrospective check). The winning model's Q4 Brier score was 0.0837 vs a 0.0853
  prevalence baseline: real but modest, which is why the recommendations rest on the
  matched comparison rather than on the model.

## Limitations

- Data is leg-level: connections and missed connections are not observable, so
  "nonstop-first for meeting-critical travel" is a stated assumption, not a finding.
- The source data contains no fares at flight level, no traveler purpose, and no
  productivity costs; cost figures are labeled scenarios with stated assumptions.
- Carrier disruption rates shown in the positioning chart are raw, not route-adjusted,
  and are not an airline league table.

## Repository map

- `Chart_Champ_Slides_Team2.pdf` - the final slide deck as presented
- `Safe_Flight_Demo_Video.mov` - the Safe Flight demo video shown in the final
- `Questrom_Chart_Champs_Revised_Brief.pdf` - the competition brief
- `Student_Package_Data/` - the official 200,000-flight student sample and documentation
- `Monthly_Flight_Data/` - full-population 2025 monthly BTS files
- `Charts/` - the presentation charts (hourly disruption, delay causes, positioning map,
  dumbbell comparison, snow impact, heatmap, buffer tail)
- `extra_data/Ticket Price Data/` - BTS DB1B ticket files used for the fare analysis
  (tracked with Git LFS)
- `extra_data/Weather Data/` - the NOAA GHCN-Daily airport weather dataset and station
  mapping built for this project

Analysis notebook: `Chart_Champ_Travel_Reliability.ipynb` in this repo (also on Google Colab, team drive).

## Sources

- Bureau of Transportation Statistics, On-Time Performance and DB1B Ticket survey, 2025
- NOAA, Global Historical Climatology Network Daily (GHCN-D), 2025
