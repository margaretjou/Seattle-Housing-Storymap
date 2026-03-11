# 🚆 Tracks to Tomorrow
### A Scrollytelling Story Map of Seattle's Link Light Rail

An interactive, scroll-driven narrative map tracing the construction of Sound Transit's Link Light Rail system, its role in catalyzing transit-oriented development (TOD), and the housing growth — and displacement pressures — that have followed in its wake.

**Live site:** 

---

## About the Project

This story map was built as a research and visualization project exploring the intersection of transit infrastructure, land use policy, and housing equity in the Seattle region. It uses real permit data and station coordinates to show how Link Light Rail has reshaped development patterns across King County since 2009.

The map walks through seven chapters:

| # | Chapter | Years |
|---|---------|-------|
| 1 | A City Decides to Connect | 2001 – present |
| 2 | Central Link Opens | 2009 |
| 3 | The TOD Awakening | 2010 – 2016 |
| 4 | North Seattle Opens Up | 2021 |
| 5 | Crossing the Lake (East Link / 2 Line) | 2024 |
| 6 | Building Near the Tracks + Rent Chart | 2015 – now |
| 7 | Before & After — Rent Near Rail (drag slider) | 2009 vs. 2023 |
| 8 | The System Completes | 2026 – 2035 |

---

## Data Sources

| Dataset | Source | File |
|---------|--------|------|
| Light Rail Stations | Sound Transit (via project CSV) | `LightRails.csv` |
| Multifamily Housing Permits | Seattle Department of Construction & Inspections | `Seattle_Housing_Dataset_cleaned_v1.csv` |
| Rail line geometry | Digitized from Sound Transit system maps | Embedded in `index.html` |

The housing dataset covers **16,000+ residential building permits** issued by the City of Seattle from 2005–2024. The map displays the **772 multifamily permits of 5+ units** to highlight larger-scale development near transit corridors.

### Research References

The contextual callout box in the Before & After chapter cites the following:

**Inflation**
- U.S. Bureau of Labor Statistics CPI-U Inflation Calculator — https://www.bls.gov/data/inflation_calculator.htm

**Employment & population demand**
- BLS Current Employment Statistics, Seattle-Tacoma-Bellevue MSA — https://www.bls.gov/eag/eag.wa_seattle_msa.htm
- Seattle Office of Planning & Community Development, 2020 Census summary — https://www.seattle.gov/opcd/population-and-demographics
- U.S. Census Bureau American Community Survey 5-year estimates (demographics of Seattle)

**Housing supply moderates rent growth**
- Monkkonen, P. et al. (2024). "Supply Skepticism Revisited." *Housing Policy Debate* (NYU Furman Center). https://www.tandfonline.com/doi/full/10.1080/10511482.2023.2205947
- Pew Charitable Trusts (2025). "More Housing Supply Is Associated With Lower Rent Growth." https://www.pewtrusts.org/en/research-and-analysis/articles/2025/01/more-housing-supply-is-associated-with-lower-rent-growth
- Gorback, C. & Keys, B. (2025). *Journal of Political Economy Macroeconomics*. https://www.journals.uchicago.edu/doi/10.1086/733547

**Walkability premium**
- Brookings Institution / Walk Score Metropolitan Policy Program research — https://www.walkscore.com/professional/research.php
- Redfin Research (18-metro walkability & home value study) — https://redfin.com/research/walkability-and-home-values

**Rent estimates**
- CoStar/BERK Consulting via Seattle *One Seattle Plan* Environmental Impact Statement (2023)
- Apartment List neighborhood rent reports (2015–2023)

---

## Project Structure

```
/
├── index.html          # Main story map (all HTML, CSS, JS in one file)
├── LightRails.csv      # Station data: name, coordinates, open date
├── Seattle_Housing_Dataset_cleaned_v1.csv   # Housing permit data
└── README.md           # This file
```

All station and housing data is embedded directly into `index.html` at build time, so the project has **zero runtime dependencies** beyond Mapbox GL JS (loaded from CDN).

---

## Map Features

- **Scrollytelling** — scroll position drives the map camera, flying between chapters automatically
- **1 Line (blue)** — accurate Central Link alignment from Federal Way to Lynnwood, including the Rainier Valley surface segment along MLK Jr Way S, Beacon Hill tunnel, downtown transit tunnel, Capitol Hill tunnel, and Portage Bay tunnel to UW
- **2 Line (gold)** — accurate East Link alignment along I-90 floating bridge from Redmond through Bellevue to International District
- **Station markers** — all 24 stations from the CSV, rendered as **diamond symbols** (white-stroked circles) to distinguish from housing data; stations **pop in progressively** as chapters advance, revealing only the stations open at each point in the story timeline
- **Map legend** — persistent overlay bottom-left showing line colors, station diamond symbols by era, and housing dot scale; housing rows appear only when housing data is visible
- **Housing heatmap** — multifamily development density, fades in at Chapter 3 (TOD Awakening)
- **Housing dots** — plain circles sized by unit count (zoom in to see), clickable for address, neighborhood, and year; visually distinct from station diamonds
- **Rent trend chart** — inline canvas chart in Chapter 5 showing avg. 1-BR rent 2009–2023 for Capitol Hill, Beacon Hill, U District, and Rainier Valley, with dashed vertical lines marking rail opening dates
- **Before/after drag slider** — Chapter 5b interactive bar chart comparing 2009 vs. 2023 rents; drag to reveal the "after" panel
- **Progress bar** — top-of-screen scroll indicator

---

## Customization

### Adding new chapters

Each chapter requires two things:

1. A `<div class="chapter">` block in the `#scroll-pane` section of `index.html`
2. A corresponding entry in the `chapters` array in the `<script>` block:

```javascript
{
  id: 'chapter-7',
  label: 'Your map label here',
  center: [-122.33, 47.61],   // [lng, lat]
  zoom: 11,
  bearing: 0,
  pitch: 30,
}
```

### Updating station data

Edit `LightRails.csv` — the file must have these columns:
```
Station_Name, Date_Opened, Latitude, Longitude
```

If you add new stations, re-run the data embedding step or manually update the `stationsGeoJSON` object in `index.html`.
