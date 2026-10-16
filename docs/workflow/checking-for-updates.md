# Checking for package updates

Use `ent` (not installed by default):
```
sudo moss install ent
```

`ent` scans the current directory tree, reads each recipe's `monitoring.yaml`, and compares its `stone.yaml` upstreams for newer versions. Scope is set by where you run it:

- **repo root** — all recipes: `gotoaosrepo && ent check updates`
- **letter dir** (e.g. `f/`) — recipes starting with that letter
- **recipe dir** (e.g. `f/firefox/`) — that single recipe
