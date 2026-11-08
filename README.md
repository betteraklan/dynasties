# Aklan Dynasty Map

An interactive map of political surname blocs in Aklan's 17 municipalities, covering every elected local official from 2001 to 2025. Styled after [BetterAklan](https://betteraklan.org) (BetterGov's Kapwa design system).

A **bloc** is the set of officials in Aklan who carry a surname as their last name *or* middle name. A shared surname is a signal, not proof of kinship.

## Features

- Choropleth of the 17 towns by share of local seats held by bloc members
- Click a town: its officials, the blocs that hold seats there, and a seat-history timeline of every mayor and vice mayor since 2001
- Click a surname: seats, via-middle-name members, votes, office mix, linked surnames, seats per election
- Click an official: full election record (votes, share, rank, result)
- Year filter (2001–2025 or all), bloc size rule (≥2, ≥3, ≥5), surname search, sortable ledger
- **Families** view: documented family trees for five Aklan political families (Miraflores-Haresco, Marquez, Calizo-Quimpo, Lachica, Navarosa, Refol, Peralta-Francisco, Legaspi, Fuentes, Maming), each link backed by public sources
- Light theme throughout (matches BetterAklan); deep links like `#lachica` or `#town-kalibo`

## Project layout

```
index.html               the whole app (HTML, CSS, JS; loads D3 and Leaflet from cdnjs)
data/aklan.js            generated dataset (window.AKLAN_DATA), ~350 KB
data/families.js         hand-curated family relationships with sources
assets/                  logo and favicon
photos/                  optional portraits for family cards (see photos/README.md)
scripts/build_data.py    rebuilds data/aklan.js from the Dynasties repo
```

## Run it locally

No build step. Open `index.html` directly in a browser, or serve the folder:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Map tiles

The background map uses Esri's World Light Gray Canvas tiles, which are free to display with attribution and need no API key. Keep the attribution visible. To use another provider, change the `L.tileLayer` URL in `index.html`.

## Rebuild the data

The dataset is extracted from the static shards in [bettergovph/officials-network-graph](https://github.com/bettergovph/officials-network-graph).

```bash
git clone --depth 1 https://github.com/bettergovph/officials-network-graph.git
python3 scripts/build_data.py officials-network-graph/public/data data/aklan.js
```

The script keeps Aklan winners for every election, the full candidacy record (including losses) of anyone who ever won, and simplified town boundaries.

## Adding a family

Edit `data/families.js`. The rules:

- **Every link needs at least one public source** (news report, official bio, encyclopedia). Put the source in `sources` and reference it by key.
- Link types: `parent` (a is parent of b), `spouse`, `sibling`, `cousin`.
- A link that follows from others (two children of one parent are siblings) is marked `derived` with a one-line reason.
- `aliases` merge ballot-name variants of one person (e.g. "Joen" and "Jo-en"). Merge only when a source ties them together.
- People with the surname but no documented link go in `unlinked`, so they are shown without implying a relationship.
- A link with strong official evidence but no source stating it in words (e.g. legal names "Sr." and "Jr.") can be added with `inferred` (the reason) and a `label`; it is drawn dashed and marked Inferred.
- `status` records changes elections can't see (resignation, removal, death), with a `label`, a `text` and sources; the person then shows as former, not in office.
- `rowfixes` move a single election row the dataset filed under the wrong person, with a source (e.g. the town's official list of mayors).
- Someone who held office before 2001 has no election record: set `"pid": null` and give a sourced `role` and `span`; the card is drawn dashed and isn't clickable.
- Layout: `gen` is the generation row (0 = top), `col` the column (fractions allowed, e.g. 0.5 to sit between two parents).

## Data and method

- Election results: Leung, Robert R. [Open Halalan: The Philippine National and Local Election Dataset](https://robertrleung.github.io/OpenHalalan/). Winners 2001–2025; vote counts from 2010 only.
- Person matching across elections: [Dynasties by BetterGov.ph](https://dynasties.bettergov.ph/) (matched by surname, first name and middle name within the province).
- Boundaries: PSGC 2023 via [philippines-json-maps](https://github.com/faeldon/philippines-json-maps).
- Poverty incidence: [Philippine Statistics Authority](https://openstat.psa.gov.ph/).

The bloc computation is a port of the Dynasties atlas logic, so figures should match [dynasties.bettergov.ph](https://dynasties.bettergov.ph/?province=aklan) for the same year and rule.

**Limitations:** common surnames group unrelated people; families that intermarry under different surnames are missed; ballot nicknames can split one person into two records.

## Credits

Contributed by Rance Ashriel R. Sabino.
