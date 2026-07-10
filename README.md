# hanta2026

Independent, source-linked tracker for the **MV Hondius Andes hantavirus cluster** (April–May 2026).

Live: https://hanta2026.org

## What it is

A single static HTML file that loads a small JSON dataset and renders:

- Case totals (cases, deaths, CFR, countries of residence)
- Ship facts (vessel, operator, route, departure/arrival, cohort size)
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
- `outbreak` — ship facts. `passengers_crew` / `passengers` / `crew` are the **full voyage cohort** (182 = 121 + 61) from the captain-signed passenger list. `aboard_at_tenerife` and its `tenerife_*` fields are the 10 May snapshot (147 = 83 + 60 + 4 medical team). Don't conflate the two.
- `cases` — per-patient line list (only includes cases with publicly-released detail). `outcome` accepts `died`, `critical`, `icu`, `recovering`; anything else renders as raw text.
- `additional_cases_note` — running dated log, appended to (never rewritten) as the story develops
- `contacts_traced.summary` — renders as the contact-tracing section lede
- `incubation_period.presymptomatic` — optional; renders as the "Infectious before symptoms" line
- `clarifications[].verdict` — first two words become a CSS class. Styled: `misleading`, `false`, `unsupported`, `speculative`, `no-evidence`, `partly-true`, `resolved`
- `polymarket.primary.yes_probability` — odds snapshot for the header pill (decimal, e.g. `0.09` = 9%). Set `resolved: "NO"` on a settled market and the percentage is replaced by "Settled NO".
- `sources` — primary source links

Status: the outbreak was declared over by WHO on **2 July 2026** (DON611). This is now a historical record, not a daily tracker.

Sources to check for updates:

- Eurosurveillance outbreak investigation (van den Berg et al., 18 Jun 2026): https://www.eurosurveillance.org/content/10.2807/1560-7917.ES.2026.31.24.2600477 — the most detailed account, and it is amended in place. Check the linked Addendum/Correction notices before trusting a figure.
- WHO Disease Outbreak News (DON611 is the end-of-outbreak notice): https://www.who.int/emergencies/disease-outbreak-news/item/2026-DON611
- ECDC: https://www.ecdc.europa.eu/en/infectious-disease-topics/hantavirus-infection/surveillance-and-updates/andes-hantavirus-outbreak
- CDC HAN00528: https://www.cdc.gov/han/php/notices/han00528.html
- NICD (South Africa), for Case 3: https://www.nicd.ac.za/hantavirus-pulmonary-syndrome-updates/
- Ministère de la Santé (France), for Case 9: https://sante.gouv.fr/actualites-presse/actualites-du-ministere/article/cas-d-hantavirus-a-bord-du-navire-mv-hondius-suivez-la-situation-en-direct

Fetch notes: WHO DON and CDC HAN pages reject some clients; send a browser User-Agent. The French ministry page sits behind a JS challenge and needs a real browser. WHO's DON index is JS-rendered, so probe `item/2026-DON<N>` directly for 200 vs 404 to test whether a newer notice exists.

## Disclaimer

Unofficial. When sources conflict, defer to WHO and ECDC, and to the peer-reviewed investigation where it is more specific. Polymarket probabilities are betting markets, not epidemiological forecasts.

## License

MIT.
