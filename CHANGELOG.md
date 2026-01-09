## main
### ✨ Features and improvements
- _...Add new stuff here..._

### 🐞 Bug fixes
- _...Add new stuff here..._

- Clone submodules during release ([#9](https://github.com/maplibre/vtvalidate/pull/9)) (by [mwilsnd](https://github.com/mwilsnd))
- Bump js version to 0.4.0 ([#8](https://github.com/maplibre/vtvalidate/pull/8)) (by [app/github-actions](https://github.com/app/github-actions))
- Add publishing CI ([#3](https://github.com/maplibre/vtvalidate/pull/3)) (by [mwilsnd](https://github.com/mwilsnd))
- Obtain vcpkg via FetchContent ([#1](https://github.com/maplibre/vtvalidate/pull/1)) (by [mwilsnd](https://github.com/mwilsnd))
- Add publishing CI ([#3](https://github.com/maplibre/vtvalidate/pull/3)) (by [mwilsnd](https://github.com/mwilsnd))
- Obtain vcpkg via FetchContent ([#1](https://github.com/maplibre/vtvalidate/pull/1)) (by [mwilsnd](https://github.com/mwilsnd))
- Clone submodules during release ([#9](https://github.com/maplibre/vtvalidate/pull/9)) (by [mwilsnd](https://github.com/mwilsnd))
- Obtain vcpkg via FetchContent ([#1](https://github.com/maplibre/vtvalidate/pull/1)) (by [mwilsnd](https://github.com/mwilsnd))
## 0.4.1
### ✨ Features and improvements

### 🐞 Bug fixes

## v0.4.0

### ✨ Features and improvements

- Migrate to shared release workflow ([#7](https://github.com/maplibre/vtvalidate/pull/7)) (by [mwilsnd](https://github.com/mwilsnd))
- Add types ([#6](https://github.com/maplibre/vtvalidate/pull/6)) (by [mwilsnd](https://github.com/mwilsnd))

## v0.3.2
- support for node 22
- removed `@mapbox/mason-js`, `@mapbox/node-pre-gyp`, `aws-sdk`, `node-addon-api`
- added `cmake-js`
- moved to submoduled dependencies

## v0.3.1
- support for node 16
- upgraded `"@mapbox/node-pre-gyp": "^1.0.8"`, `"aws-sdk": "^2.1074.0"`, `"node-addon-api": "^4.3.0"`, `"@mapbox/mvt-fixtures": "~3.6.0"`, `"aws-sdk": "2.1074.0"`, `"bytes": "^3.1.2"`,`"d3-queue": "^3.0.7"`, `"minimist": "~1.2.5"`, `"tape": "^5.5.0"`

## v0.3.0

- N-API (`node-addon-api`)
- Binaries are now compiled with clang 10.x
- Updated mason and node_modules
- Upgrade to `mapbox/node-pre-gyp@1.x`

## v0.2.3

- Add `-Wno-error=deprecated-declarations` flags to binding.gyp to turn NAN errors into warnings [#27](https://github.com/mapbox/vtvalidate/issues/27)
- Add package-lock.json

## v0.2.2

- Added node v8 and node v10 support

## v0.2.1

- Update bench test to handle errors properly per https://github.com/mapbox/vtvalidate/pull/19
- Changes: https://github.com/mapbox/vtvalidate/compare/v0.2.0...v0.2.1

## v0.2.0

- Add gzip-hpp compression support per https://github.com/mapbox/vtvalidate/pull/18
- Add CLI
- Changes: https://github.com/mapbox/vtvalidate/compare/v0.1.0-alpha1...v0.2.0

## v0.1.0-alpha1
- Initial release based on node-cpp-skel
- Add memory stats option to bench tests
