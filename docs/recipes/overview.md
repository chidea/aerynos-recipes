# Recipe overview

A recipe is metadata describing a package plus the *instructions* to build it reproducibly, enabling automated builds and updates. `stone.yaml` resembles other packaging systems.

Basic recipe (nano):

```yaml
name      : nano
version   : 8.7
release   : 38
homepage  : https://www.nano-editor.org/
upstreams :
  - https://www.nano-editor.org/dist/v8/nano-8.7.tar.xz : <sha256>
summary   : GNU Text Editor
description : |
    Nano is a small and simple text editor for use on the terminal.
license   :
  - GPL-3.0-or-later
builddeps :
  - binary(msgfmt)
  - pkgconfig(libmagic)
  - pkgconfig(ncursesw)
  - pkgconfig(zlib)
setup   : |
  %configure
build   : |
  %make
install : |
  %make_install
```

Simple format, but `boulder` hides a lot of power (macros, providers, auto-splitting).
