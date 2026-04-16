# RouteSync POI Database

Pre-generated POI tile data for the RouteSync cargo bike route planner.
Covers Southeast Europe, Turkey, and surrounding regions.

## Structure

```
tiles/           1x1 degree grid tiles (GeoJSON-like JSON)
manifest.json    Tile index with metadata, categories, timestamps
```

## Tile Format

Each tile covers a 1x1 degree area (`latMin` to `latMin+1`, `lonMin` to `lonMin+1`).

```json
{
  "id": "42-20",
  "latMin": 42, "lonMin": 20,
  "latMax": 43, "lonMax": 21,
  "generated": "2026-04-16T...",
  "categories": ["charging","camping","water",...],
  "count": 342,
  "pois": [
    {
      "type": "node",
      "id": 123456,
      "lat": 42.5,
      "lon": 20.3,
      "tags": { "amenity": "charging_station", "name": "..." }
    }
  ]
}
```

## Coverage

- **Balkans**: Albania, Bosnia, Bulgaria, Croatia, Greece, Kosovo, Montenegro,
  North Macedonia, Romania, Serbia, Slovenia
- **Turkey**: Full coverage including all regions
- **Extended**: Austria, Hungary, Slovakia, southern Czech Republic, southern Poland

## Categories (19)

charging, camping, sightseeing, shopping, fuel, water, bicycle_repair,
hospital, pharmacy, restaurant, cafe, hostel, parking, hotel, atm, bakery,
bicycle_parking, picnic_site, viewpoint

## Usage in RouteSync

The POI aggregator checks this database before hitting the Overpass API:

```
Phase 0:  IndexedDB Cache  (<1s, local)
Phase 0.5: GitHub Tiles    (~1-2s, CDN)
Phase 1:  Overpass API     (rate-limited, slow)
Phase 2:  Local files      (static backup)
```

Tiles are fetched via `cdn.jsdelivr.net` and cached in IndexedDB for offline use.
After the first load, subsequent searches use the cached version.

## Statistics

| Metric | Value |
|--------|-------|
| Tiles | 48 |
| Total POIs | 74,674 |
| Categories | 19 |
| Total Size | ~14.9 MB |
| Geographic Coverage | Lat 34°–43°, Lon 15°–44° |
| Generation Date | 2026-04-15 |

## Regeneration

See `.github/workflows/regenerate.yml` for the weekly auto-regeneration pipeline.
Manual regeneration:

```bash
npx tsx scripts/generate-poi-database.ts \
  --bbox 34,13,47,45 \
  --output RouteSync-POI-Data/tiles \
  --delay 3000
```

## Data Source

All POI data is sourced from [OpenStreetMap](https://www.openstreetmap.org/)
via the [Overpass API](https://wiki.openstreetmap.org/wiki/Overpass_API).
Data is licensed under the [ODbL](https://opendatacommons.org/licenses/odbl/).

## Version

Generated for RouteSync v1.5.2
