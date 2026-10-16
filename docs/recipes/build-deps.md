# Build dependencies

Every build gets a fresh minimal root; add what's needed via `builddeps`. Packages can store **providers**, making these dependency kinds work.

| Kind            | Purpose                                                        |
| --------------- | -------------------------------------------------------------- |
| `$name`         | Dependency on a package name (discouraged; providers are more resilient) |
| `binary(name)`  | An executable in `/usr/bin` (e.g. `binary(grep)`)               |
| `sysbinary(name)` | An executable in `/usr/sbin` (e.g. `sysbinary(mount)`)         |
| `pkgconfig(name)` | Map to a pkgconfig `.pc` file (e.g. `pkgconfig(ncurses)`)      |
| `pkgconfig32(name)` | 32-bit `.pc` in `/usr/lib32/pkgconfig`                        |
| `cmake(name)`   | CMake module name (e.g. `cmake(Qt5OpenGL)`)                    |
