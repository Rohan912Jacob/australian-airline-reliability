# Australian Airline Reliability

A Power BI portfolio project analysing Australian domestic flight punctuality, cancellations and routes to and from Perth using public BITRE data.

**Author:** Rohan Varghese Jacob  
**Tools:** Power BI Desktop, Power Query (M), DAX  
**Analysis period:** January 2022–September 2026

## Project purpose

This report explores three questions:

- How does domestic airline punctuality change over time?
- How do airlines compare on on-time arrivals and cancellations?
- Which Perth routes have the lowest on-time arrival rates in the selected period?

## Report sections

| Section | Contents |
|---|---|
| Introduction | Project purpose, navigation, metric definitions and source information |
| Flight performance | Scheduled-flight cards, cancellation and punctuality rates, monthly chart, airline and year filters |
| Airline comparison | Airline rankings by on-time arrivals and cancellations, with flight volumes in tooltips |
| Perth routes | Route-level table, five least punctual routes and a year filter |

## Dataset

Source: **Bureau of Infrastructure and Transport Research Economics (BITRE), Domestic Airline On-Time Performance**.

[Official dataset and time-series downloads](https://www.bitre.gov.au/resource/aviation/airline-time-performance-monthly-reports-and-time-series-data)

The supplied CSV contains monthly airline and route statistics, including scheduled flights, flights flown, cancellations, on-time arrivals and departures, and delayed arrivals and departures. Each retained record represents an airline operating a directional route in a particular month.

The analysis uses **20,266 records across 57 months** after removing summary rows and restricting the period to January 2022 onwards. The supplied snapshot ends in September 2026; **2026 is a partial year**.

## Data preparation

Power Query was used to:

1. Import the CSV and promote its headers.
2. Assign text, date and whole-number data types.
3. Exclude `All Airlines` summary rows.
4. Exclude rows where either airport is `All Ports` to avoid counting totals alongside detailed records.
5. Retain dates from 1 January 2022 onwards.

A calculated column identifies records where either the departing or arriving airport is Perth. Routes remain directional: Sydney–Perth and Perth–Sydney are analysed separately.

## Measures

Rates are calculated from summed flight counts, rather than averaging row-level percentages.

| Metric | Calculation |
|---|---|
| Scheduled flights | Sum of `Sectors_Scheduled` |
| Flights flown | Sum of `Sectors_Flown` |
| Cancelled flights | Sum of `Cancellations` |
| Cancellation rate | Cancelled flights ÷ scheduled flights |
| On-time arrival rate | On-time arrivals ÷ flights flown |
| On-time departure rate | On-time departures ÷ flights flown |

Example DAX:

```DAX
Scheduled Flights =
SUM(FlightPerformance[Sectors_Scheduled])

Flights Flown =
SUM(FlightPerformance[Sectors_Flown])

Cancelled Flights =
SUM(FlightPerformance[Cancellations])

Cancellation Rate =
DIVIDE([Cancelled Flights], [Scheduled Flights])

On-Time Arrival Rate =
DIVIDE(
    SUM(FlightPerformance[Arrivals_On_Time]),
    [Flights Flown]
)
```

Create each measure separately in Power BI. `DIVIDE` returns a blank when the denominator is zero unless an alternative result is specified.

## Selected findings

These results were independently calculated from the filtered CSV. Report values change with the selected year, airline or route.

| Scope and period | Scheduled flights | On-time arrival rate | Cancellation rate |
|---|---:|---:|---:|
| All retained routes, Jan 2022–Sep 2026 | 1,972,962 | 72.8% | 3.3% |
| Routes to/from Perth, Jan 2022–Sep 2026 | 260,577 | 72.8% | 2.2% |
| Routes to/from Perth, 2023 | 54,131 | 70.2% | 2.8% |

For the complete analysis period, Sydney–Perth had an on-time arrival rate of **64.9%**, compared with **72.4%** for Perth–Sydney. This illustrates why the two directions are retained separately. The difference alone does not establish its cause.

## Validation

Checks on the filtered CSV found:

- No duplicate airline–route–month keys.
- No missing values in the retained records.
- Scheduled flights equalled flights flown plus cancellations on every row.
- On-time arrivals plus delayed arrivals equalled flights flown on every row.
- On-time departures plus delayed departures equalled flights flown on every row.

The Perth 2023 totals were also compared with the report's displayed results and matched after rounding.

## Open the report

1. Download `Australian Airline Reliability.pbix` and `otp_time_series_web.csv` from this repository.
2. Open the `.pbix` file in Power BI Desktop.
3. Use the report tabs to explore the analysis and the available slicers to select years or airlines.
4. Hover over charts for additional details.

GitHub does not provide an interactive preview of the `.pbix` report. Download it to explore the visuals in Power BI Desktop.

### Refresh the CSV connection

The report uses a local CSV connection. To refresh it on another computer:

1. Save `otp_time_series_web.csv` in a local folder.
2. Open **Transform data → Data source settings** in Power BI Desktop.
3. Select the CSV source and use **Change Source** to point to your local copy.
4. Apply the change and refresh the report.

If the source cannot be changed through that dialog, update the file path in the query's `Source` step. Keep the expected CSV column names and structure unchanged.

## Interpretation and limitations

- The report covers the routes and airlines in the source dataset, not necessarily every domestic flight.
- Airlines serve different route networks and may have different reporting periods. Rankings are descriptive, not like-for-like comparisons.
- January–September 2026 flight totals should not be compared directly with full-year totals without matching the months.
- Year selections may differ between report pages; check each page's slicer before comparing results.
- A month-name-only axis combines corresponding months across selected years. For a chronological trend, use the original `Month` date field rather than only the month level of its date hierarchy.
- The dataset does not establish why a flight was delayed or cancelled. Weather, congestion and other causes cannot be inferred from these figures alone.

## Skills demonstrated

- Cleaning and filtering public data with Power Query.
- Writing DAX measures with appropriate denominators.
- Building interactive cards, charts, tables and slicers.
- Applying Bottom N filtering and measure-based sorting.
- Validating data consistency and communicating analytical limitations.

## Attribution

Data is provided by BITRE. This is an independent portfolio project and is not an official BITRE publication. Refer to the source publisher for methodology and applicable data-use terms.
