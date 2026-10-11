# Park & Keep airport parking index (Europe)

Car parks serving European airports — shuttle, valet, covered parking, distance — and their **public
prices for 3, 7 and 14-day stays, read on each operator's own website**. Published monthly by
[Park & Keep](https://parkandkeep.com) under **CC BY 4.0**.

- Data page: https://parkandkeep.com/en/data/airport-parking-index
- Page en français : https://parkandkeep.com/donnees/indice-parkings-aeroport
- Files: [`data/airport-parking-index-2026-10.csv`](data/airport-parking-index-2026-10.csv) (one row per car park) and
  [`data/airport-parking-index-2026-10.json`](data/airport-parking-index-2026-10.json) (same rows nested, plus one
  aggregate row per airport). Frictionless descriptor: [`datapackage.json`](datapackage.json).

## In this release

<!-- stats:start -->
Snapshot **2026-10**, generated on 2026-10-11 (values computed from the JSON by `sync.mjs`):

| | |
|---|---|
| Airports | 46 (8 countries: AT, BE, CH, DE, ES, FR, IT, NL) |
| Car park listings | 521 |
| Listings with at least one price | 61 |
| Price amounts (3, 7 and 14-day stays) | 171 |
| Airports with a 3 / 7 / 14-day aggregate (at least 3 prices) | 11 / 11 / 11 |
| Currencies | CHF, EUR |
| Prices read between | 2026-10-11 and 2026-10-11 |
| Listings verified between | 2026-09-01 and 2026-09-03 |
| Reading methods present | html_text, quote |
<!-- stats:end -->

## Method

- **Where prices come from.** Every amount is read on the operator's own pages: the rate table of its website
  (`price.method` = `html_text:…`) or a quote from its own booking engine for a dated stay (`quote:<engine>`, with
  `price.stay_start`). Never on an aggregator or comparison site. `robots.txt` is respected.
- **Traceability.** Each priced listing carries the page it was read on (`price.source_url`) and the date it was read
  (`price.checked_at`). An amount is kept only if the amount and the stay duration appear word for word in an excerpt
  captured from that page; amounts that fail this check, mixed currencies or non-monotonic rate tables go to manual
  review instead of the file.
- **What is not a price.** `null` means not collected; it never means free. `price.method` = `-` means the page was
  checked and no usable price was found (prices on request, login wall, rates without a duration…).
- **Aggregates.** Per airport and duration, min / median / max are published only when at least 3 prices exist
  (`airports[].prices.d3|d7|d14`, otherwise `null`). Prices are public list prices at the date read; they change with
  season, demand and promotions, and are not quotes.
- **Listings.** A listing is a car park listed for that airport in the Park & Keep comparator; `last_verified` is the
  date it was last checked. Phone numbers, review scores and any affiliate or partner link are excluded from the export.
- Full method: https://parkandkeep.com/en/airport-parking/how-we-rank

## Field dictionary

<!-- fields:start -->
Read from `data/airport-parking-index-2026-10.json` (types and fill rates computed on the file).

**`listings[]`** — one row per car park (the CSV has the same rows, nested keys flattened):

| JSON path | CSV column | Type | Filled | Description |
|---|---|---|---|---|
| `id` | `id` | string | 521 / 521 | Stable identifier of the listing in the Park & Keep comparator. |
| `airport.iata` | `airport_iata` | string | 521 / 521 | IATA code of the airport the car park serves. |
| `airport.city` | `airport_city` | string | 521 / 521 | Airport name as shown on Park & Keep. |
| `airport.country` | `airport_country` | string | 521 / 521 | ISO 3166-1 alpha-2 country code of the airport. |
| `airport.vertical` | `airport_vertical` | string | 521 / 521 | Comparator section (AIRPORT = airport parking). |
| `name` | `name` | string | 521 / 521 | Name of the operator or car park. |
| `city` | `city` | string, nullable | 495 / 521 | Town of the car park. |
| `postal_code` | `postal_code` | string, nullable | 442 / 521 | Postcode of the car park. |
| `lat` | `lat` | number, nullable | 463 / 521 | Latitude (WGS 84). |
| `lng` | `lng` | number, nullable | 463 / 521 | Longitude (WGS 84). |
| `distance_km` | `distance_km` | number, nullable | 463 / 521 | Distance from the car park to the airport, in kilometres. |
| `attributes.shuttle` | `shuttle` | boolean, nullable | 409 / 521 | Shuttle to the terminal offered (null = not stated by the operator). |
| `attributes.valet` | `valet` | boolean, nullable | 272 / 521 | Valet / meet-and-greet offered (null = not stated). |
| `attributes.covered` | `covered` | boolean, nullable | 278 / 521 | Covered or indoor parking offered (null = not stated). |
| `attributes.open_24h` | `open_24h` | boolean, nullable | 376 / 521 | Open or accessible 24 hours a day (null = not stated). |
| `attributes.keys_kept` | `keys_kept` | boolean, nullable | 168 / 521 | Customer leaves the car keys with the operator (null = not stated). |
| `attributes.ev_charging` | `ev_charging` | boolean, nullable | 115 / 521 | Electric vehicle charging available (null = not stated). |
| `attributes.online_booking` | `online_booking` | boolean, nullable | 502 / 521 | Online booking available (null = not stated). |
| `attributes.online_payment` | `online_payment` | boolean, nullable | 255 / 521 | Online payment available (null = not stated). |
| `website` | `website` | string, nullable | 520 / 521 | Website of the operator. |
| `last_verified` | `last_verified` | date | 521 / 521 | Date the listing (name, location, attributes) was last checked. |
| `price.d3` | `price_d3` | number, nullable | 57 / 521 | Public price of a 3-day stay, in price.currency (null = not collected). |
| `price.d7` | `price_d7` | number, nullable | 61 / 521 | Public price of a 7-day stay, in price.currency (null = not collected). |
| `price.d14` | `price_d14` | number, nullable | 53 / 521 | Public price of a 14-day stay, in price.currency (null = not collected). |
| `price.currency` | `price_currency` | string, nullable | 112 / 521 | ISO 4217 currency of the prices of this listing. |
| `price.source_url` | `price_source_url` | string, nullable | 112 / 521 | Page of the operator (website or its own booking engine) where the prices were read. |
| `price.checked_at` | `price_checked_at` | date, nullable | 156 / 521 | Date the price page was read (or the listing was last passed to the survey). |
| `price.stay_start` | `price_stay_start` | datetime, nullable | 36 / 521 | Start of the simulated stay for prices obtained as a booking quote (null for rate tables). |
| `price.method` | `price_method` | string, nullable | 156 / 521 | How the prices were read: html_text:<kind> = text of the operator's rate table; quote:<engine> = quote from the operator's own booking engine; "-" = page checked, no usable price; null = not surveyed. |

**`airports[]`** — one row per airport, aggregates computed over its listings (JSON only):

| JSON path | Type | Filled | Description |
|---|---|---|---|
| `iata` | string | 46 / 46 | IATA code of the airport. |
| `city` | string | 46 / 46 | Airport name as shown on Park & Keep. |
| `country` | string | 46 / 46 | ISO 3166-1 alpha-2 country code. |
| `vertical` | string | 46 / 46 | Comparator section (AIRPORT = airport parking). |
| `parkings` | integer | 46 / 46 | Number of listings at this airport in the file. |
| `share_shuttle` | number | 46 / 46 | Share of the airport's listings with shuttle = true (null counts as no). |
| `share_valet` | number | 46 / 46 | Share of the airport's listings with valet = true (null counts as no). |
| `share_covered` | number | 46 / 46 | Share of the airport's listings with covered = true (null counts as no). |
| `distance_km_median` | number | 46 / 46 | Median distance to the airport of the listings with a known distance, km. |
| `distance_km_min` | number | 46 / 46 | Shortest distance to the airport among the listings, km. |
| `prices_collected` | integer | 46 / 46 | Number of listings at this airport with at least one price. |
| `prices.d3` | object, nullable | 11 / 46 | Aggregate of the 3-day prices (null when fewer than 3 prices). |
| `prices.d7` | object, nullable | 11 / 46 | Aggregate of the 7-day prices (null when fewer than 3 prices). |
| `prices.d14` | object, nullable | 11 / 46 | Aggregate of the 14-day prices (null when fewer than 3 prices). |
| `prices.*.min` | number | 33 / 33 | Lowest price of the duration. |
| `prices.*.median` | number | 33 / 33 | Median price of the duration. |
| `prices.*.max` | number | 33 / 33 | Highest price of the duration. |
| `prices.*.n` | integer | 33 / 33 | Number of prices behind the aggregate. |
<!-- fields:end -->

## Use and citation

Licence: [Creative Commons Attribution 4.0](LICENSE). Reuse, including commercial, is free provided you credit
**Park & Keep** with a link to https://parkandkeep.com/en/data/airport-parking-index. Citation metadata:
[`CITATION.cff`](CITATION.cff).

Related: a Go package embedding this index (`github.com/tresor4k/airport-parking-index-go`) and a remote MCP server
that answers questions about it (`io.github.tresor4k/parkandkeep-mcp`).

Corrections (an operator, a price, a closed car park): open an issue, or write via https://parkandkeep.com.

---

## En bref (français)

Indice ouvert des parkings d'aéroport en Europe publié chaque mois par Park & Keep, sous licence CC BY 4.0 (citer
« Park & Keep » avec un lien vers https://parkandkeep.com/donnees/indice-parkings-aeroport).

<!-- stats-fr:start -->
Instantané **2026-10**, généré le 2026-10-11 (valeurs calculées depuis le JSON par `sync.mjs`) : 46 aéroports dans 8 pays, 521 fiches de parkings, 61 fiches avec au moins un prix, 171 montants relevés (séjours de 3, 7 et 14 jours), prix lus entre le 2026-10-11 et le 2026-10-11. Agrégats min / médiane / max publiés seulement à partir de 3 prix (3 / 7 / 14 jours : 11 / 11 / 11 aéroports).
<!-- stats-fr:end -->

Méthode : chaque prix est relevé sur le site de l'exploitant lui-même (grille tarifaire ou moteur de réservation de
l'exploitant), jamais sur un agrégateur ; chaque fiche chiffrée porte l'URL de la page source et la date du relevé, et
un montant n'est conservé que si le montant et la durée figurent mot pour mot dans l'extrait relevé sur cette page.
`null` = non relevé (jamais « gratuit »). Le dictionnaire des champs ci-dessus vaut pour le CSV comme pour le JSON.
