# Prerequisites

Set up an aerynOS system to build package recipes.

## Install `build-essential`

```
sudo moss sync -u
sudo moss it build-essential
```

## Activate the helper scripts

Clone the recipes repo, then symlink the helpers script so `gotoaosrepo` / `chpkg` are available in new shells:

```
git clone https://github.com/aerynOS/recipes
ln -sv ~/Projects/aerynos/recipes/tools/helpers.bash ~/.bashrc.d/90-aerynos-helpers.bash
cd ~
gotoaosrepo   # verify it switches to the recipes repo dir
```

## Git hooks and linters

`just` (installed with `build-essential`) drives the hooks:

```
gotoaosrepo
just init      # installs prepare-commit-msg + pre-commit hooks, commit templates
```

## Optional git niceties

- **`manifest.*.bin` diff** — add to the repo `.git/config` under `[core]`:
  ```
  [diff "moss"]
      textconv = moss inspect
      binary = true
  ```
  (`.gitattributes` already declares the `moss` filter.)
- **`git gone` alias** — in `~/.gitconfig`, delete local branches whose upstream is gone.

## `/etc/subuid` and `/etc/subgid`

`boulder` uses user-namespaces for isolated buildroots, so subuid/subgid ranges must be allocated for the build users (root + your user) before building.
