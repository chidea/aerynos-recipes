# Metadata

Metadata for discovery and automation; strongly-typed keys enable auto update checking.

## Mandatory keys

- **`name`** — upstream source name, as close to upstream as possible. Used as the basename when subpackages are auto-generated (e.g. `zlib` → `zlib`, `zlib-devel`, `zlib-dbginfo`).
- **`version`** — freeform string, not used for comparison logic. Should match the upstream identifier so new releases are detected automatically.
- **`release`** — monotonically incrementing integer. Bump it to schedule a new build; without it no build is scheduled.
- **`homepage`** — upstream web presence.
- **`license`** — string or list of [SPDX](https://spdx.org) identifiers for all applicable licenses.
