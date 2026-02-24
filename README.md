# Vector Extension Specification

- **Title:** Vector
- **Identifier:** <https://stac-extensions.github.io/vector/v0.1.0/schema.json>
- **Field Name Prefix:** vector
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @m-mohr

This document explains the Vector Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.
It defines basic properties and metrics of vector data (as in: geometries with additional properties).
This extension works well in conjunction with the [Table Extension](https://github.com/stac-extensions/table) for tabular / columnar datasets.

- Examples:
  - [Item example](examples/item.json)
  - [Collection example](examples/collection.json)
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Fields

The fields in the table below can be used in these parts of STAC documents:

- [ ] Catalogs
- [x] Collections
- [x] Item Properties (incl. Summaries in Collections)
- [x] Assets (for both Collections and Items, incl. Item Asset Definitions in Collections)
- [x] [Table Column](https://github.com/stac-extensions/table?tab=readme-ov-file#column-object)
- [ ] Links

| Field Name             | Type      | Description                                                                   |
| ---------------------- | --------- | ----------------------------------------------------------------------------- |
| vector:geometry_types  | \[string] | A list of the geometry types present in the dataset.                          |
| vector:mmu             | number    | Minimum Mapping Unit - the smallest polygon area represented.                 |
| vector:mmw             | number    | Minimal Mapping Width - the smallest real-world width represented as polygon. |
| vector:reference_scale | integer   | Representative fraction denominator (e.g., 50000 for 1:50,000).               |

### Additional Field Information

#### vector:geometry_types

Lists the GeoJSON geometry types present in the described dataset.

The list MUST contain each geometry type at most once and MAY be in any order.

MUST be one of the GeoJSON geometry types:

- `Point`
- `MultiPoint`
- `LineString`
- `MultiLineString`
- `Polygon`
- `MultiPolygon`
- `GeometryCollection`

#### vector:mmu

Minimum Mapping Unit (MMU) is the smallest polygon area represented in the dataset.

- Use this to communicate the effective minimum area of polygonal features after generalization.
- Unit MUST be square meters ($m^2$).
- The value MUST be greater than 0.

Example: If a land-cover dataset does not include patches smaller than 100 $m^2$, set `vector:mmu` to `100`.

#### vector:mmw

Minimum Mapping Width (MMW) is the smallest real-world width that is still represented as an area (polygon) in the dataset.

- Use this to communicate the effective minimum width of mapped polygonal features (e.g., narrow rivers/roads represented as polygons).
- Unit MUST be meters ($m$).
- The value MUST be greater than 0.

Example: If narrow rivers below 2 meters are not mapped as polygons (or are mapped as lines instead), set `vector:mmw` to `2`.

#### vector:reference_scale

Representative fraction denominator (also known as the reference scale), expressed as an integer.

- Example: `50000` represents a reference scale of 1:50,000.
- The value MUST be greater than 0.
- This is intended to describe the cartographic/generalization level of the dataset and is not the same as a map display zoom level.

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes are valid.
To run tests locally, you'll need `npm`, which is a standard part of any [node.js installation](https://nodejs.org/en/download/).

First you'll need to install everything with npm once. Just navigate to the root of this repository and on 
your command line run:

```bash
npm install
```

Then to check markdown formatting and test the examples against the JSON schema, you can run:

```bash
npm test
```

This will spit out the same texts that you see online, and you can then go and fix your markdown or examples.

If the tests reveal formatting problems with the examples, you can fix them with:

```bash
npm run format-examples
```
