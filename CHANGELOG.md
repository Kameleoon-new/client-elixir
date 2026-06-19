# Changelog

All notable changes to this project will be documented in this file.
## 0.9.0 - 2026-06-19
### Features
* Implemented performance and stability improvements.

### Bug fixes
* Removed the redundant `details` field from `Kameleoon.Error`.
* Made the `cookie_accessor` parameter of [`set_legal_consent`][set_legal_consent] optional.
* Renamed the `timeout_ms` parameter of [`initialized`][initialized] to `timeout`.

[initialized]: https://docs.kameleoon.com/developer-docs/sdks/web-sdks/elixir-sdk#initialize
[set_legal_consent]: https://docs.kameleoon.com/developer-docs/sdks/web-sdks/elixir-sdk#set_legal_consent

## 0.8.6 - 2026-06-17
### Patch Changes
* Dependency versions are now pinned to exact versions (`=`) instead of using compatible version ranges (`^`). Affected libraries:
  - [kameleoon-core][kameleoon-core]

[kameleoon-core]: https://crates.io/crates/kameleoon-core



## 0.8.5 - 2026-06-16
### Features
* Initial release of the `kameleoon_client` package.
