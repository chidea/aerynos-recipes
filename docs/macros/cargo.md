# cargo macros

For "pure" Rust builds; use the `%cargo*` macros so `boulder` controls tuning/debuginfo.

| Macro                          | Description                              |
| ------------------------------ | ---------------------------------------- |
| `%cargo_set_environment`       | Set env vars for the Cargo build         |
| `%cargo_fetch`                 | Fetch dependencies                       |
| `%cargo_vendor`                | Vendor all dependencies                  |
| `%cargo_build`                 | Build to `%(cargo_target_dir)`           |
| `%cargo_install`               | Install artefacts from `%(cargo_target_dir)` |
| `%cargo_test`                  | Run tests                                |
| `%cargo_home_breaks_this_build` | Mark that `CARGO_HOME` breaks this build   |

Usage: `environment: | %cargo_set_environment`.
