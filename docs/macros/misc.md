# Miscellaneous macros

| Macro                     | Description                                                     |
| ------------------------- | --------------------------------------------------------------- |
| `%export_docbook_catalogs`| Make docbook builds find XML catalogs locally                    |
| `%gtk_update_icon_cache`  | Pregenerate the icon cache at build time                         |
| `%install_bin <file>`     | Install a file to `%(bindir)`                                    |
| `%install_dir <dir>`      | Create an empty directory with default permissions               |
| `%install_exe <src> <dst>`| Install a file with executable permissions                       |
| `%install_file <src> <dst>`| Install a file without executable permissions                   |
| `%patch <file> [-pN]`     | Patch upstream sources (default `-p1`)                           |
| `%tmpfiles`               | Create a tmpfiles.d file for the package                         |
| `%sysusers`               | Create a sysusers.d file for the package                         |
