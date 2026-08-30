# RadReady derived datasets — ODbL share-alike

`NuclearFacilities.json` is a **Derivative Database** of OpenStreetMap, distributed
inside the RadReady iOS app. ODbL 1.0 §4.6 obliges anyone who publicly uses a
derivative to offer recipients a machine-readable copy of it. This directory is
that offer.

## NuclearFacilities.json

253 nuclear power and research reactor sites worldwide.

**Licence:** [Open Database License (ODbL) v1.0](https://opendatacommons.org/licenses/odbl/1-0/)
**Source:** © [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors
**Extracted:** 30 August 2026

### How it was derived

Fetched from the Overpass API:

```
[out:json][timeout:150];
(
  nwr["power"="plant"]["plant:source"="nuclear"];
  nwr["disused:power"="plant"]["plant:source"="nuclear"];
  nwr["man_made"="reactor"];
);
out center tags;
```

Then, in order:

1. Dropped objects with no `name` / `name:en` and no resolvable coordinate.
   Ways and relations were reduced to their `center`.
2. Derived `status` from OSM tags rather than trusting a single field:
   `disused:*` / `disused=yes` / `abandoned=yes` / an `end_date` in the past →
   `closed`; a `construction:*` tag or a future `start_date` → `construction`;
   otherwise `operating` if `start_date` is present, else `unknown`.
3. Parsed capacity in MW from the first of `plant:output:electricity`,
   `disused:plant:output:electricity`, `construction:plant:output:electricity`
   or `generator:output:electricity` that matched `<number> (MW|GW)`.
4. Rounded coordinates to five decimal places.
5. De-duplicated on coordinates rounded to two decimals, keeping the entry with
   the largest capacity.
6. Dropped null fields and sorted by name.

No data was added from any other source, and no values were edited by hand.

### Fields

| field | meaning |
|---|---|
| `name` | `name:en` where present, else `name` |
| `lat`, `lon` | WGS84, 5 dp |
| `status` | `operating` · `closed` · `construction` · `unknown` |
| `kind` | `power` · `reactor` |
| `operator` | OSM `operator:en` or `operator` |
| `megawatts` | electrical capacity, MW |
| `startYear`, `endYear` | from OSM `start_date` / `end_date` |
| `wikidata` | Wikidata Q-id where OSM records one |

### Accuracy

Spot-checked against reality when built: Chornobyl 51.390 / 30.104, closed 2000 ·
Fukushima I 37.423 / 141.030, closed 2014 · Three Mile Island 40.153 / −76.724,
closed 2019 · Zaporizhzhia 47.510 / 34.588, operating, 5,700 MW.

Operating status comes from OpenStreetMap and can lag real-world changes. National
nuclear regulators are authoritative.
