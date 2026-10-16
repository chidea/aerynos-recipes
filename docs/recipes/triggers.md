# Triggers

Triggers are actions that run at the end of a package install. After `moss` composes the filesystem and builds the staging tree for the new `/usr`, any trigger files under `/usr/share/moss/triggers` are loaded; **matching** triggers run at the appropriate stage.

Trigger logic uses `glob`-style path matches and is **not incremental** — designed to avoid running arbitrary scripts, instead matching patterns to handlers.

## Capturing globs

Named capture groups via `*`/`?` (brace support planned):

```
/usr/lib/(GROUP_NAME:PATTERN)/dir
```
The `(...)` is a non-greedy capture group named `GROUP_NAME` matching `PATTERN`. E.g.
```
/usr/share/icons/(name:*)/index.theme
```
matches `/usr/share/icons/hicolor/index.theme` and sets `name=hicolor`. Captures are substituted into handler args, so handler execution is controlled without interim scripts.

See [Transaction triggers](tx-triggers.md) for the transactional scope.
