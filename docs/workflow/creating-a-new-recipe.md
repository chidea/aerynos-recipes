# Creating a new package recipe

Example uses `nano`; the steps apply to any new package. First confirm no outstanding PR or package-request issue exists for it.

## Scaffold

```
gotoaosrepo
mkdir -p n/nano
cd n/nano
```

## Generate the skeleton

```
boulder recipe new "<upstream url>"
```
For nano: `boulder recipe new https://www.nano-editor.org/dist/v8/nano-8.7.tar.xz`

This creates `stone.yaml` + `monitoring.yaml`, auto-fills what it can (name/version/release/homepage), and records the source **sha256**.

## Fill in the recipe (steps)

1. **Collect upstream metadata** — latest release + canonical download link; note build prerequisites (candidates for `builddeps`).
2. **Core metadata** — correct `homepage`, `summary`, `description`.
3. **License** — find it upstream (`COPYING`/`LICENSE`), convert to an SPDX identifier.
4. **Build deps** — map upstream prerequisites to aerynOS package names; use `pkgconfig(<lib>)` for `.pc`-providing libs. Toolchain (gcc, make) is in the sandbox — don't list it.
5. **Build steps** — use the standard macros for the build system (`%configure`/`%make`/`%make_install` for autotools).
6. **Review** the finished `stone.yaml`.
