# Changelog

All notable changes to this project will be documented in this file.
## 0.9.3 - 2026-10-09
### Features
* The native library is now always compiled in release mode, regardless of the host application's `MIX_ENV`. SDK developers can opt into debug builds via `KAMELEOON_ELIXIR_NIF_MODE=debug`.
* Long-running native calls (client creation, client removal, datafile snapshot) are now scheduled on dirty schedulers and no longer stall BEAM scheduler threads.
* Added ExDoc documentation for the public API: `Kameleoon.Client` functions (including option units and defaults), data structs (`Kameleoon.Data.*`), result types (`Kameleoon.Types.*`) and `Kameleoon.Error`. The changelog is now shipped with the package.

### Bug fixes
* [`ClientFactory.create/2`][create] now returns an `Initialization` error when the site code is empty or when the client ID or client secret is empty, consistent with every other client-creation failure. The error message names the exact cause. The `SiteCodeIsEmpty` and `ConfigCredentialsInvalid` error codes previously used for these cases are no longer returned.
* Asynchronous operations ([`initialize`][initialize], `flush_instant`, `get_remote_data`, `get_remote_visitor_data`, `get_visitor_warehouse_audience`) no longer time out prematurely after 30 seconds and no longer leave stray messages in the caller's mailbox. Timeouts are governed by the configured timeouts only.
* The SDK is now an OTP application: its events process runs under `Kameleoon.Supervisor` and is restarted automatically after a crash. Configuration-update handlers, SDK event handlers and the custom logger survive such a crash: the restarted process restores them, and the native library addresses it by name instead of by pid. No setup is required from host applications, as Mix starts dependency applications automatically.
* [`ClientFactory.forget/2`][forget] without `:environment` no longer drops the handlers of clients created with an environment.
* The deprecated `on_datafile_update/2` is now a shorthand for `set_event_handler/3` with `:datafile_update`. Its callback is thereby also protected against crashes like every other event handler.
* Configuration file read or parse errors are now reported by `ClientFactory.create/2` instead of being silently ignored.
* Removed the implicit default configuration file path `/etc/kameleoon/client-elixir.conf`. `ClientFactory.create/2` now requires either `:config` or `:config_path` and returns an `Initialization` error when neither is given, instead of failing on an unexpected file.
* Data structs built without `new!/2` and carrying an unsupported `:type` (for example `%Kameleoon.Data.Browser{type: :netscape}`), or a `Kameleoon.Data.CustomData` with neither `:index` nor `:name`, are now rejected with the new `InvalidArgument` error code instead of `Internal`.
* `get_datafile/1` now returns an `Initialization` error until the client is ready, instead of an empty data file.
* Lowered the log level of the message emitted when the streaming server closes the real-time (SSE) connection from `error` to `info`. The streaming server recycles connections periodically and the SDK reconnects automatically, so this is expected behavior. Connection failures (error responses, network errors) are still logged as `error`.

[create]: https://docs.kameleoon.com/developer-docs/sdks/web-sdks/elixir-sdk#create
[forget]: https://docs.kameleoon.com/developer-docs/sdks/web-sdks/elixir-sdk#forget
[initialize]: https://docs.kameleoon.com/developer-docs/sdks/web-sdks/elixir-sdk#initialize

## 0.9.2 - 2026-09-11
### Bug fixes
* Reduced the load on the Data API during its outages. After a failed tracking request, the SDK sends only one visitor's data per tracking request until a request succeeds, instead of retrying with a full-size (up to 2.5 MB) request on every tracking interval. Once the Data API is reachable again, the accumulated visitor data is sent in full-size requests as usual.

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
