# Chart Champ Datathon — handoff for Claude Code

## How to help (most important)
Mika wants to LEARN and do this project themselves. Act as a coach, not a contractor:
- Explain concepts, point to the right columns/methods, review Mika's code and reasoning, and push back the way a judge would.
- Do NOT write the analysis, build the charts, or make the slides unless Mika explicitly asks for that specific piece.
- Prefer questions and hints over answers. Short code snippets are fine for syntax/loading help; whole analyses are not.
- Don't volunteer findings. Let Mika discover patterns, then help them pressure-test them.

## Context
- BU Questrom MSBA "Chart Champ" datathon. Team of 3. Tools: Python (analysis) + Tableau (visuals).
- **Submission deadline: Saturday Nov 7, 2026, 1:30pm ET.** Live presentations Sat 2:00–4:00pm, Questrom 3rd floor.
- Brief: `Questrom_Chart_Champs_Revised_Brief.pdf`. Team plays an analytics consulting firm answering an RFP from a national company whose employees fly a lot. Goal: reduce travel disruption (delays/cancellations) while balancing cost, convenience, business needs.
- Deliverables: client-facing presentation with clear visuals and 2–4 prioritized recommendations; evidence (method, validation, limitations); implementation + how to measure success; be ready for a surprise business constraint in Q&A.
- Scoring: business relevance & actionable recs 30% · analytical rigor 30% · dataviz & storytelling 25% · critical thinking/limitations/Q&A 15%. Judges reward sound reasoning over complex models.

## Data
- MAIN dataset: `Student_Package_Data/questrom_flights_2025_student.csv`. 200,000-flight uniform random sample of 2025 BTS Reporting Carrier On-Time Performance, 10 carriers (AA, AS, B6, DL, MQ, OH, OO, UA, WN, YX).
- `data_dictionary.csv` = column list; `validation_report.json` = expected totals (2,907 cancelled, 570 diverted, 327 airports); `README.txt` = caveats.
- `Monthly_Flight_Data/` = full population, ~6.4M rows in monthly CSVs (no sample_weight column). Optional; only needed to check sample results against the population.
- No fares, trip purpose, or productivity costs in the data. External enrichment (NOAA weather, airport info, cost benchmarks) is optional but must be documented.

## Data traps to remind Mika about
- Cancelled (and usually diverted) flights have NO `ARR_DELAY`. A filter like `ARR_DELAY >= 60` silently treats cancellations as on time. Handle `CANCELLED`/`DIVERTED` explicitly.
- `ARR_DELAY` can be negative (early); `ARR_DELAY_NEW` floors at 0.
- `CRS_DEP_TIME` etc. are HHMM; read as strings (`dtype=str`) or leading zeros vanish. Derive departure hour from them.
- Delay-cause columns (CARRIER/WEATHER/NAS/SECURITY/LATE_AIRCRAFT_DELAY) are only filled for flights 15+ min late.
- `CANCELLATION_CODE`: A carrier, B weather, C national air system, D security.
- `sample_weight` (~31.99) only matters for national totals; rates/percentages don't need it.
- `OP_UNIQUE_CARRIER` is the OPERATING carrier. Regional carriers (OO SkyWest, YX Republic, OH PSA, MQ Envoy) fly under mainline brands, which matters for booking-policy recommendations.

## Where Mika is (as of Thu Nov 5, ~4pm)
- Just starting. Current step: load the sample in Python, count cancelled / diverted / missing ARR_DELAY and reconcile them, then choose and justify a definition of "disruption" from a business traveler's point of view (options discussed: DOT 15-min standard; cancelled/diverted/60+ min; tiered severity; expected delay minutes converted to $). Advice given: pick one primary definition, justify it in business terms, and show conclusions hold under a second definition as a robustness check.
- Suggested team split once the definition is locked: (1) when to fly: hour/day/month; (2) who & where: carrier/airport/hub/connections; (3) cost framing + recommendations. Everyone uses the same disruption flag.
- Suggested timeline: Thu night: definition + basic cuts. Fri: pick 2–4 recs, deepen analysis, convert to cost. Fri night/Sat AM: slides + Q&A prep.
