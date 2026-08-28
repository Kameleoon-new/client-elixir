# Changelog

All notable changes to this project will be documented in this file.
## 0.9.1 - 2026-08-28
### Features
* [`initialize`][initialize] now fail fast when the initial configuration fetch has failed: instead of waiting out the timeout, they immediately return the actual fetch error, such as a network or HTTP failure. A later successful fetch (for example, on the next polling cycle) still makes the client ready, and subsequent calls return successfully. The behavior of [`is_ready`][is_ready] is unchanged: it returns `true` only after a configuration has been successfully fetched, and never reverts afterwards.
* Added support for SDK event handlers through the new [`set_event_handler`][set_event_handler] API:
  - `:datafile_update` notifies when the SDK data file (configuration) is updated with [polling](https://docs.kameleoon.com/developer-docs/feature-experimentation/technical-reference/technical-considerations#polling-default) or [streaming](https://docs.kameleoon.com/developer-docs/feature-experimentation/technical-reference/technical-considerations#streaming-premium-option) modes.
  - `:http_request` notifies when SDK HTTP requests complete successfully or fail.
  - HTTP request events include the request type, HTTP status or failure details, and request duration.
  - Passing `nil` to `set_event_handler` clears the handler for the selected event type.
* The [`on_datafile_update`](https://docs.kameleoon.com/developer-docs/sdks/web-sdks/elixir-sdk#on_datafile_update) method has been deprecated in favor of `set_event_handler` with the `:datafile_update` event type.
### Bug fixes
* Targeting conditions of an unknown type now evaluate to `false` instead of `true`. Previously an unsupported condition matched every visitor; now it matches none.

[initialize]: https://docs.kameleoon.com/developer-docs/sdks/web-sdks/elixir-sdk#initialize
[is_ready]: https://docs.kameleoon.com/developer-docs/sdks/web-sdks/elixir-sdk#is_ready
[set_event_handler]: https://docs.kameleoon.com/developer-docs/sdks/web-sdks/elixir-sdk#set_event_handler

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
