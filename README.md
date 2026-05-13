# hanta2026

Independent, source-linked tracker for the **MV Hondius Andes hantavirus cluster** (April–May 2026).

Live: https://hanta2026.org

## What it is

A single static HTML file that loads a small JSON dataset and renders:

- Case totals (cases, deaths, CFR, countries of residence)
- Ship facts (vessel, operator, route, departure/arrival)
- Stylised SVG map of the ship's Atlantic route with case events
- Sortable case line list (per-patient detail from WHO DON599)
- Polymarket "hantavirus pandemic 2026" odds snapshot
- Primary sources (WHO, ECDC, CDC, Wikipedia)

## Design constraints

- One `index.html` with inline CSS and JS — no frameworks, no build step, no external CDNs.
- Data lives in `data.json`, fetched at load.
- Page weight ~28 KB; first paint sub-100 ms on a fast connection.
- Dark theme, serif headers, monospace numbers, amber accent. Designed to look unlike the generic AI-generated COVID-tracker template.
- Anonymous pageview counts via Cloudflare Web Analytics (no cookies, no tracking IDs, no personal data, no third-party advertisers).

## Hosting

GitHub Pages, custom domain `hanta2026.org`. Free.

## Updating the data

Edit `data.json` and commit. The relevant fields:

- `last_updated` — ISO timestamp shown in the header
- `totals` — top-line case/death numbers
- `cases` — per-patient line list (only includes cases with publicly-released detail)
- `additional_cases_note` — narrative summary for cases without released detail
- `polymarket.primary.yes_probability` — odds snapshot for the header pill (decimal, e.g. `0.09` = 9%)
- `sources` — primary source links

Sources to check for updates:

- WHO Disease Outbreak News: https://www.who.int/emergencies/disease-outbreak-news/item/2026-DON599
- ECDC: https://www.ecdc.europa.eu/en/infectious-disease-topics/hantavirus-infection/surveillance-and-updates/andes-hantavirus-outbreak
- CDC HAN00528: https://www.cdc.gov/han/php/notices/han00528.html

## Disclaimer

Unofficial. When sources conflict, defer to WHO and ECDC. Polymarket probabilities are betting markets, not epidemiological forecasts.

## License

MIT.
