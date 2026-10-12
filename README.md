# Open Hazard Map

An open, locale-agnostic **GeoJSON data standard** and a free, open-source **reference Progressive Web App (PWA)** for mapping traffic hazards on children's routes to school.

> **Status: early stage / work in progress.** The schema below is a draft and will change. No stable release yet.

## Why

Children walking, cycling or riding scooters to school face hazards (unmarked crossings, missing sidewalks, blind corners, speeding traffic) that parents and teachers know locally but that are rarely recorded in a structured, shareable way. Aggregate crash statistics exist, but there is no shared open data format for hyper-local, child-specific hazard information that any school or municipality can adopt, regardless of country or budget.

Open Hazard Map aims to provide that shared layer:

1. **A proposed open data standard (primary goal):** a versioned GeoJSON schema (`HazardPoint`, `SafeRoute`) with controlled enumerations instead of free text, so it can be used unchanged in any language or country.
2. **A reference implementation:** an installable, offline-capable PWA that shows hazards on a map, lets anyone report a hazard anonymously, and gives moderators a simple panel to verify reports.

The first pilot site is Trstenik, Serbia. Nothing in the schema or the app is specific to Serbia.

This is a *proposed draft standard* from an independent author. It is not an officially ratified European standard.

## Design principles

- **Open and free:** no proprietary map APIs, no license fees, no vendor lock-in.
- **Facts separate from opinion:** objectively observable attributes (marked crossing, sidewalk, lighting, posted speed limit) are stored separately from subjective, citizen-rated severity, plus a `verified_by_authority` flag. Different countries can apply their own criteria on a shared factual base.
- **Child data protection and digital safety:** no personal data is collected, reports are anonymous, no photos in the first version (avoids location metadata leaks), hazard points describe roads and never homes or individual routes, and each record has a `visibility` level (`public`, `school_only`, `municipality_only`) enforced by role-based access control.
- **Security basics:** HTTPS only, input validation, rate limiting, least-privilege moderator access, regular dependency updates and backups, and a published privacy and security note.
- **Local-first:** the map and hazard data are cached on the device and work offline.
- **OpenStreetMap-friendly:** built on OpenStreetMap data and tagging conventions, with the aim of proposing useful extensions back to the OSM community.

## Draft schema (illustrative, subject to change)

```json
{
  "type": "Feature",
  "geometry": { "type": "Point", "coordinates": [20.9966, 43.6181] },
  "properties": {
    "schema_version": "0.1",
    "hazard_type": "unmarked_crossing",
    "affected_road_user": ["pedestrian", "cyclist", "scooter"],
    "severity": 3,
    "has_marked_crossing": false,
    "has_sidewalk": true,
    "speed_limit_kmh": 50,
    "verified_by_authority": false,
    "visibility": "public",
    "status": "pending_review",
    "reported_at": "2026-10-12T12:00:00Z"
  }
}
```

Hazard types and other categorical fields will use controlled, translatable enumerations. A JSON Schema file for validation will be published with the first versioned release.

## Planned technology

- PostgreSQL + PostGIS
- Node.js, Express, TypeScript
- React, Vite, Tailwind CSS
- Leaflet with OpenStreetMap-based tiles
- Service worker for offline use

All components are free and open source.

## Roadmap

1. **Standard definition:** versioned GeoJSON schema, JSON Schema validation files, documentation, example datasets, mapping to OSM tags.
2. **Reference app, map:** offline-capable PWA with a read-only map of hazard points.
3. **Reporting and moderation:** anonymous reporting, moderator panel, PostGIS backend, validated GeoJSON export.

Later ideas (not committed): school pilot and moderator hand-over, educational material, proposing schema extensions to OpenStreetMap.

## Contributing

Feedback on the schema is especially welcome, from road-safety practitioners, schools, municipalities, OpenStreetMap contributors and developers. Please open an issue to discuss ideas or problems.

## License

- **Code:** [MIT](LICENSE)
- **Schema documentation:** CC-BY 4.0

## Author

Vladan Čolić, Trstenik, Serbia.
