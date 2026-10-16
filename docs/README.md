# aerynOS Packaging docs

Summaries of the [aerynOS packaging documentation](https://aerynos.com/packaging/) — workflow, recipes (`stone.yaml`), and build macros.

## Workflow

The end-to-end packaging process on aerynOS.

- [Prerequisites](workflow/prerequisites.md) — install `build-essential`, enable helpers, set up subuid/subgid.
- [Basic packaging workflow](workflow/basic-workflow.md) — local repo, build profiles, resolution order.
- [Preparing for packaging](workflow/preparing-for-packaging.md) — update clone, create branch, one commit per package.
- [Creating a new recipe](workflow/creating-a-new-recipe.md) — scaffold, `boulder recipe new`, fill metadata/build steps.
- [Updating an existing recipe](workflow/updating-an-existing-recipe.md) — bump release, `boulder recipe update`.
- [Building and testing packages](workflow/building-and-testing-packages.md) — `just build`, `just mv-local`, `moss install`.
- [Using git](workflow/using-git.md) — hooks, commit message format, common commands.
- [Submitting a PR](workflow/submitting-a-pr.md) — push, PR description format.
- [After submitting a PR](workflow/after-submitting-a-pr.md) — review → build queue → volatile → unstable.
- [Checking for updates](workflow/checking-for-updates.md) — `ent check updates`.

## Recipes

The `stone.yaml` format and `monitoring.yaml`.

- [Overview](recipes/overview.md) — a basic recipe example.
- [Upstreams](recipes/upstreams.md) — plain (tarball + SHA256) and git sources.
- [Metadata](recipes/metadata.md) — mandatory keys: name, version, release, homepage, license.
- [Monitoring](recipes/monitoring.md) — release tracking (Anitya id, RSS) and security (CPE).
- [Build dependencies](recipes/build-deps.md) — `binary()`, `pkgconfig()`, `cmake()`, etc.
- [Package definitions](recipes/package-definition.md) — root package, subpackages, overriding defaults.
- [Triggers](recipes/triggers.md) — install-time actions, capturing globs.
- [Transaction triggers](recipes/tx-triggers.md) — run post-blit in a container.
- [System accounts](recipes/system-accounts.md) — stateless userdb; [Groups](recipes/groups.md) / [Users](recipes/users.md).

## Macros

Build-system action/definition macros available in every recipe.

- [autotools](macros/autotools.md) — `%configure`, `%make`, `%make_install`, …
- [bazel](macros/bazel.md) — `%bazel_fetch`, `%bazel_build`.
- [cargo](macros/cargo.md) — `%cargo_build`, `%cargo_install`, …
- [ccache](macros/ccache.md) — `%ccache_zero`, `%ccache_stats`.
- [cmake](macros/cmake.md) — `%cmake`, `%cmake_build`, `%cmake_install`, …
- [golang](macros/golang.md) — `%go_mod`.
- [meson](macros/meson.md) — `%meson`, `%meson_build`, `%meson_install`, …
- [Miscellaneous](macros/misc.md) — `%patch`, `%install_file`, `%install_dir`, …
- [ninja](macros/ninja.md) — `%ninja`, `%ninja_install`, `%ninja_test`.
- [perl](macros/perl.md) — `%perl_setup`, `%perl_module_*`.
- [pgo](macros/pgo.md) — LLVM/BOLT profile-guided optimization.
- [python](macros/python.md) — `%python_setup`, `%pyproject_build`, …
- [qt-kde](macros/qt-kde.md) — `%qmake_qt5`, `%cmake_qt6`, `%cmake_kf6`.
- [ruby](macros/ruby.md) — `%gem_build`, `%gem_install`.
