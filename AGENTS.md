<!--
# SPDX-FileCopyrightText: 2026 aerynOS Developers
# SPDX-License-Identifier: MPL-2.0
-->

# AGENTS.md

Operating notes for packaging tasks in this repo (aerynOS `stone.yaml` recipe repository).
Hard-won know-how from reverse-engineering + rebuilding packages. The full packaging reference lives in `docs/` — start at `docs/README.md` (workflow, `stone.yaml` format, build macros); consult it for recipe fields, macros, and the PR process.

## Before packaging

- **Approval gate.** New packages need an approved package-request issue; unapproved PRs can be rejected. Self-triage against `PACKAGING_POLICY.md` acceptance criteria first: SPDX-compatible license, freely redistributable, ≥6 months since first stable release with a release in the past year, low stack complexity (few new deps, clean stateless integration), clear value to aerynOS, sustainable maintenance. Don't build work that will be rejected.
- **GenAI policy.** The repo does not accept contributions authored by LLM/agent chatbots. A human reviews, owns, and submits every change.
- **Branch discipline.** Work on a branch off updated `main`; one commit per package. A package plus its new dependencies is several commits — each dependency its own. See `docs/workflow/preparing-for-packaging.md`.
- **Hooks.** `just init` symlinks `tools/prepare-commit-msg.py` and `tools/pre-commit.py` into the git hooks; run it once per clone.

## Workflow (run from the package dir)

`just` wraps boulder/moss. The justfile reads `.env` overrides: `BOULDER`, `BOULDER_ARGS`, `BOULDER_PROFILE` (default `local-x86_64`), `LOCAL_REPO` (default `~/.cache/local_repo/x86_64`).

- `just build` — build `stone.yaml` in the current directory
- `just bump` — bump `release`
- `just chroot` — shell into the build chroot (heredoc into it) to inspect compiler/env/headers
- `just mv-local` — move the built `.stone` into the local repo and re-index
- `just ls-local`, `just rm-local <stone>`, `just clean-local`, `just clean` — local-repo and artifact housekeeping
- `just updates` — `ent check updates` for upstream releases
- `just verify-reuse` — `reuse lint`
- `just check-monitoring-fields`, `just check-monitoring-missing` — tree-wide monitoring audits

Test loop: `just build` → `just mv-local` → `sudo moss repo enable local` (once) → `sudo moss sync -u` → `sudo moss install <name>` → smoke-test the installed program.

`moss it <name>` installs **by name, not file path**.

Inspecting a built `.stone`: `moss inspect <file.stone>` (metadata: deps, providers, layout); `moss extract <file.stone> -o <dir>` — stone files contain **content only, no embedded recipe**. Reverse-engineer recipes from deps + file layout.

## Committing

Commit **from the recipe dir** so the hooks see the recipe. `prepare-commit-msg` prefills the title:

- `name: Add at v<version>` — first release
- `name: Update to v<version>` — version changed
- `[NFC] name: <description>` — `stone.yaml` changed but not rebuilt (no new manifest)
- `name: Fix <...>` / `name: <description>` — other recipe changes

Stage the recipe artifacts: `git add stone.yaml manifest.x86_64.bin manifest.x86_64.jsonc monitoring.yaml`, plus `pkg/` for patches and extra files. `.stone` files are not committed.

Review feedback: stay on the PR branch and amend the affected package's existing commit — never a second commit for the same package. After pushing: `git push --force-with-lease`.

`chpkg`, `fixup-recipe-commit`, `gotoaosrepo` are shell functions defined in `tools/helpers.bash`; they are not available to non-interactive shells. Use plain `cd` + `git add` + `git commit --amend`.

## Lint before pushing

`pre-commit` runs `tools/CI/package_checks.py` on staged files; CI runs the same checks plus file-conflict detection. Run it locally before committing: `tools/CI/package_checks.py <files>`. Checks: homepage, `monitoring.yaml` exists + format, package bumped, build-dep order, package directory name, version, SPDX license, unwanted files, manifest. CI: `.github/workflows/package_lint.yml` and `conflicts.yaml` (duplicate files across packages — the usual cause of `-devel` file conflicts).

## Recipe layout (per package dir `x/<name>/`)

- `stone.yaml`, `monitoring.yaml`, `pkg/` (patches, applied via `%patch %(pkgdir)/<name>.patch` in `setup`), `manifest.x86_64.{bin,jsonc}` (boulder artifacts, tracked in git).
- Bump `release` on any rebuild. Upstream URLs need a trailing sha256: `- <url> : <sha256>`. `monitoring.yaml`: `releases: {id: <github repo id>, rss: <releases.atom>}` + `security: cpe:`. SPDX headers on all files (repo uses MPL-2.0, REUSE policy applies). Field details: `docs/recipes/`.
- `manifest.x86_64.bin` is a binary blob with a custom git diff driver (`diff=moss`); never hand-edit — regenerate by rebuilding. Parse the jsonc manifest with `cpp -P -E manifest.x86_64.jsonc | jq .packages` (jq can't read jsonc).
- `boulder build -o <dir>` sets where `.stone` + manifests are emitted (default `.`).
- This checkout is a **partial clone** (only some packages per letter dir); a package missing locally may still exist upstream — check `https://raw.githubusercontent.com/AerynOS/recipes/main/<letter>/<name>/stone.yaml`.

## Rebuild cascades

Bumping a shared library triggers `name: global rebuild` commits for the dependent packages — that is the dominant commit type in the log. ABI/soname changes require rebuilding everything linking the library.

## Package splitting

- boulder auto-splits files into subpackages from templates in `/usr/share/boulder/macros/arch/base.yaml`: `%(name)` (catch-all `*`), `%(name)-devel` (`/usr/include`, `/usr/lib/*.a`, `/usr/lib/cmake`, `/usr/lib/pkgconfig`, ...), `%(name)-libs`, `%(name)-dbginfo`, etc.
- The `-devel` template outranks the main `*` catch-all, so headers / static libs / cmake / pkgconfig auto-land in a `%(name)-devel` subpackage.
- To keep devel files in the **main** package (for `*-devel` / `*-devel-static` packages that *are* the devel package), add a `packages:` entry named `%(name)` (same as the main package) listing the devel paths. Sub-packages are added last → highest rule priority, and the name collision merges into the main package, so the auto `-devel` subpackage ends up empty and is dropped.
- `%install_file <src> <dest>` installs a single file (no exec perms). Use it to ship only one artifact (e.g. `%(builddir)/lib/libfoo.a` → `%(installroot)%(libdir)/libfoo.a`) instead of the full `%cmake_install` — this avoids file conflicts when the main package already provides the headers / `.pc` / cmake files.

## Build environment facts

- chroot `clang++` defaults to **libstdc++** (CXXFLAGS carry `-Wp,-D_GLIBCXX_ASSERTIONS`); aerynOS C++ packages conventionally link libstdc++. Do **not** add a `libcxx` builddep unless a recipe genuinely needs libc++ headers.
- C++ builddeps are `pkgconfig(<lib>)` entries (e.g. `pkgconfig(gnutls)`); toolchain (clang, libstdc++) comes with the chroot, not builddeps.
- `boulder build` warns "version and first parsed upstream version do not match" when the URL's tag can't be parsed as the version (e.g. `release-1.37.0` tags). Cosmetic; `boulder recipe update` relies on `monitoring.yaml`, not the URL.
- **No network in the chroot**: DNS + TCP both fail. CMake `FetchContent` / build-time downloads fail — bundle dep sources locally (extract into `%(builddir)`, point CMake at them via `*_SOURCE_DIR` vars) or use in-repo builddeps.
- **`bsdtar`, not `tar`**: the chroot has `bsdtar` (libarchive) but no `tar`; `bsdtar -xzf … --strip-components=1` uses the same syntax.
- `%(builddir)` is a **relative** path (the CMake out-of-source build dir); `mkdir -p %(builddir)/…` in `setup` to stage extra deps there before `%cmake`.

## Gotchas

- **Stale moss repo cache**: moss caches repo indexes in `/.moss/repo/<uri-hash>/` (root-owned). After re-indexing the local repo, `moss info`/`moss it` still resolve the **old** index until `moss repo update local` — which needs **root** (non-root gets `Permission denied` on `/.moss`). Verify what a name resolves to with `moss info <name>`; a fresh local index alone does not change resolution for non-root.
- `moss la` / `moss info` show the state's available-package DB, not the live local index.
- Match the original package's file layout when reverse-engineering (e.g. no locale `.mo` files ⇒ original used `--disable-nls`).
- `moss inspect` output: "N entries blitted" counts **directories too**; compare with `find -type f` for real file counts.
- Library-only builds need no builddeps for optional features gated by cmake options that only affect the CLI programs (program/contrib/test flags vs library flags) — read upstream's `CMakeLists.txt` to see which options gate what.
- Not every package ships a separate `-devel` subpackage; check what the built package actually provides (`moss inspect`) before assuming one exists.

## Removals

`removals.md` lists packages to delete once moss supports package removal; each entry **must** list all subpackages the package creates. Removal requests use the `request_package_removal` issue template. `shared-control-files/` holds boulder control files shared across recipes (e.g. `disable-tests.kdl`, which empties the `check` phase).
