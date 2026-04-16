# RouteSync POI Database

Pre-generated POI tile data for the RouteSync cargo bike route planner.
Worldwide coverage with 80+ countries across all continents.

## Structure

```
tiles/           2x2 degree grid tiles (Overpass-compatible JSON)
manifest.json    Tile index with metadata, categories, timestamps
```

## Tile Format

Each tile covers a 2x2 degree area (`latMin` to `latMin+2`, `lonMin` to `lonMin+2`).
Tiles contain Overpass-compatible elements directly usable by the RouteSync POI aggregator.

```json
{
  "id": "52-13",
  "latMin": 52, "lonMin": 13,
  "latMax": 54, "lonMax": 15,
  "generated": "2026-04-16T...",
  "count": 42,
  "pois": [
    {
      "type": "node",
      "id": 12345,
      "lat": 52.5,
      "lon": 13.4,
      "tags": { "amenity": "charging_station", "name": "..." }
    }
  ]
}
```

## Coverage

Worldwide — 80+ countries on all continents. Top regions by POI density:

- **Europe**: Germany, France, UK, Netherlands, Norway, Sweden, Italy, Spain, Switzerland, Austria, Poland, Czech Republic, Finland, Denmark, Belgium, Ireland, Portugal, Hungary, Romania, Greece, Croatia, Slovenia, Slovakia, Baltic States, Balkans, Turkey
- **North America**: USA (all states), Canada, Mexico
- **Asia**: China, Japan, South Korea, India, Thailand, Singapore, Taiwan, Indonesia, Malaysia, Vietnam
- **Oceania**: Australia, New Zealand
- **South America**: Brazil, Chile, Argentina, Colombia, Uruguay, Ecuador
- **Africa**: South Africa, Kenya, Morocco, Egypt, Nigeria
- **Middle East**: UAE, Saudi Arabia, Israel, Qatar, Bahrain, Oman

## Categories (7)

| Category | Description |
|----------|-------------|
| `charging` | Electric vehicle charging stations (Type 2, CCS, CHAdeMO) |
| `fuel` | Petrol/diesel/gas fuel stations |
| `restaurant` | Restaurants, fast food, cafes |
| `supermarket` | Supermarkets, grocery stores, convenience stores |
| `pharmacy` | Pharmacies and drugstores |
| `hospital` | Hospitals, clinics, emergency medical facilities |
| `bicycle_repair` | Bicycle repair shops and service stations |

## Usage in RouteSync

The POI aggregator checks this database before hitting the Overpass API:

```
Phase 0:  IndexedDB Cache  (<1s, local)
Phase 0.5: GitHub Tiles    (~1-2s, CDN)   <- THIS REPO
Phase 1:  Overpass API     (rate-limited, slow)
Phase 2:  Local files      (static backup)
```

Tiles are fetched via `cdn.jsdelivr.net` from this dedicated repository and cached
in IndexedDB for offline use. After the first load, subsequent searches use the
cached version.

## Statistics

| Metric | Value |
|--------|-------|
| Tiles | 3,019 |
| Total POIs | 186,081 |
| Categories | 7 |
| Total Size | ~48.6 MB |
| Geographic Coverage | Worldwide (80+ countries) |
| Top Operator | Tesla (4,000+ stations) |
| Generation Date | 2026-04-16 |

## CDN Access

This repo is served via jsDelivr CDN for fast global access:

```
https://cdn.jsdelivr.net/gh/nicetoTECHyou/RouteSync-POI-Data@main/manifest.json
https://cdn.jsdelivr.net/gh/nicetoTECHyou/RouteSync-POI-Data@main/tiles/poi-tile-{lat}-{lon}.json
```

## Data Source

All POI data is sourced from [OpenStreetMap](https://www.openstreetmap.org/)
via the [Overpass API](https://wiki.openstreetmap.org/wiki/Overpass_API).
Data is licensed under the [ODbL](https://opendatacommons.org/licenses/odbl/).

## Version

Generated for RouteSync v1.5.5 — POI Data v1.1.0
