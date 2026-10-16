# Basic packaging workflow

How `boulder`/`moss` use a **local moss-format repo** to build and test packages.

## Local repo

- `~/.cache/local_repo/x86_64/` holds `.stone` files + a `stone.index`.
- Create and index it:
  ```
  gotoaosrepo
  just create-local    # makes the empty local repo dir
  just index-local     # writes stone.index
  ```

## Register the local repo

**boulder** — add a `local-x86_64` build profile with the official `volatile` repo (priority 0) plus the local index (priority 100):

```
boulder profile add \
  --repo name=volatile,base-uri=https://cdn.aerynos.dev,version=stream/volatile,priority=0 \
  --repo name=local,uri=file://${HOME}/.cache/local_repo/x86_64/stone.index,priority=100 \
  local-x86_64
```
Saved to `~/.config/boulder/profile.d/local-x86_64.kdl`.

**moss** — add the same repos:
```
sudo moss repo add unstable https://cdn.aerynos.dev --root-index channel=main,version=stream/unstable -p 0
sudo moss repo add volatile https://cdn.aerynos.dev --root-index channel=main,version=stream/volatile -p 10
sudo moss repo add local file://${HOME}/.cache/local_repo/x86_64/stone.index -p 100
```

## Resolution order

Highest priority wins: `local` (100) → `volatile` (10) → `unstable` (0, official upstream).

- **Disable**: `sudo moss repo disable <name>` (keep definitions, e.g. `volatile` is intentionally off by default).
- **Enable**: `sudo moss repo enable <name>` (needed temporarily when testing local builds, since `local-x86_64` resolves against `volatile`).

## Build with `local-x86_64`

Recommended first target: `nano`.
```
gotoaosrepo
chpkg nano
just bump      # bump release
git diff
just build     # build with the local-x86_64 profile
```
