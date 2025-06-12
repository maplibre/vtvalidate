[![MapLibre Logo](https://maplibre.org/img/maplibre-logo-big.svg)](https://maplibre.org/)

# vtvalidate

Validate vector tiles based on the [Mapbox Vector Tile Specification 2.x](https://www.mapbox.com/vector-tiles/specification/) via [vtzero](https://github.com/mapbox/vtzero).

## Build & Test

`npm install && npm run test`

## Usage
 
- If the tile is valid, `vtvalidate` will return an empty string. 
- If the tile is invalid, `vtvalidate` will return the string output from vtzero. 
- `vtvalidate` will throw an error if there is unexpected behaviour, for example an invalid argument value passed into `vtvalidate.isValid()` or a corrupt compressed buffer.

#### Valid tile
```js
var vtvalidate = require('@maplibre/vtvalidate');

...

// Pass in protocol buffer (uncompressed)
vtvalidate.isValid(buffer, function(err, result) {
  if (err) throw err;

  // returns empty string if it's a valid tile
  console.log(result); // ''
});
```

#### Invalid tile
```js
var vtvalidate = require('@maplibre/vtvalidate');

...

// Pass in protocol buffer (uncompressed)
vtvalidate.isValid(buffer, function(err, result) {
  if (err) throw err;

  // returns string that specifies why the tile is invalid
  console.log(result); // 'Missing geometry field in feature (spec 4.2)'
});
```

#### Type of validation
`vtvalidate` validates tile data against [vtzero](https://github.com/mapbox/vtzero):
- Tile data consistent with the [Mapbox vector tile spec - Version 2](https://www.mapbox.com/vector-tiles/specification/). Tiles created via Mapbox vector tile spec Version 1 will be flagged invalid.
- Read tile layer(s) and feature(s)
- Decode properties
- Decode geometries

Currently, vtvalidate does not check geometries for self-intersections. If you'd like to add more extensive tile validation, check out [this example](https://github.com/mapbox/vtzero/blob/master/examples/vtzero-check.cpp).


## CLI

Accepts either uncompressed or gzip/zlib compressed tiles

```
node bin/vtvalidate.js <path-to-vector-tile>
```
Will output:
- empty string if it's a valid tile
- string that specifies why the tile is invalid


## Bench
Provide desired iterations and concurrency
```
node bench/isValid.bench.js --iterations 50 --concurrency 10
```
