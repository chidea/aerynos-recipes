# What happens after submitting a recipe PR

1. **Reviewed + merged** — maintainers review the recipe, metadata, and PR checks.
2. **Built** — the merged recipe enters the build queue (may wait on build deps). Follow it at [dash.aerynos.dev](https://dash.aerynos.dev/); failed builds stay visible with logs.
3. **Published to `volatile`** — new packages land in the volatile stream first for testing. May be delayed by dependency chains.
4. **Promoted to `unstable`** — maintainers sync the tested state to unstable; no extra PR needed from contributors.

Path:
```
merged recipe PR
      ↓
build queue / dependency builds
      ↓
volatile stream
      ↓
unstable stream
```
