# Package definitions

A single recipe can produce multiple packages via automatic splitting. Certain subpackages are predefined in `boulder` for consistent names/splitting; others are explicitly defined in the recipe. Each recipe also has a **root package definition** (the default target), merged with the standard metadata.

## Package metadata

- **`summary`** — one-line description.
- **`description`** — in-depth description (from README).
- **`rundeps`** — manually specified runtime dependencies; e.g. make one split package depend on another or add a hard dependency not captured by the automatic system:
```yaml
rundeps:
  - "%(name)-devel"
  - filesystem
```

## Defining a subpackage

Extend the `packages` set and match paths:
```yaml
packages:
  - "%(name)-tools":
    summary: Cool tools package
    description: |
        Provides a cool set of tools!
    paths:
      - /usr/bin/extra-tool
```
Auto dependencies and providers still work for subpackages.

## Overriding defaults

- Keep headers in the root (skip `-devel`) for a headers-only package: `paths: [/usr/include]`.
- Add to a predefined package:
```yaml
packages:
  - "%(name)-docs":
    paths:
      - /usr/share/custom-docs
```
