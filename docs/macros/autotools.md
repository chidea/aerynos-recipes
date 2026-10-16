# autotools macros

For projects with a `Makefile` and/or `./configure`.

| Macro                 | Description                                                  |
| --------------------- | ------------------------------------------------------------ |
| `%autogen`            | Run `autogen.sh` with dash; attempts to run `./configure` once |
| `%autogen_with_bash`  | Same, with GNU Bash                                          |
| `%configure`          | `./configure` with default options, dash (dash starts ~20% faster than bash) |
| `%configure_with_bash`| `./configure` with default options, GNU Bash                 |
| `%make`               | `make` the current Makefile                                  |
| `%make_no_verbose`    | `make` without the VERBOSE flag                              |
| `%make_install`       | `make install` to the destination directory                  |
| `%reconfigure`        | Re-configure without `autogen.sh`, dash                      |
| `%reconfigure_with_bash` | Re-configure without `autogen.sh`, GNU Bash               |
