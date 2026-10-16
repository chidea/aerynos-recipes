# Updating an existing package recipe

Example uses `nano`. First confirm no outstanding update PR/issue exists.

## Navigate

```
chpkg nano
```

## Simple updates

- **Bump release only** (dependency changed, package unchanged): `just bump` (= `boulder recipe bump`). Increments `release` by 1 — don't run it repeatedly for one update.
- **Update the version**:
  ```
  boulder recipe update --ver 8.7 --upstream https://www.nano-editor.org/dist/v8/nano-8.7.tar.xz stone.yaml -w
  ```
  Updates version + upstream URL, re-checks the sha256, and bumps release by 1.

## Wider updates

Edit `stone.yaml` manually (see *Creating a new package recipe* for field guidance).

## Build, test, commit

Build and test per *Building and testing packages*. Commit **one commit per package** (see *Using git* and *Submitting a PR*).
