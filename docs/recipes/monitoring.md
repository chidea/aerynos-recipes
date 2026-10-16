# Monitoring

Every recipe ships a `monitoring.yaml` so tooling watches upstream releases and security issues. Two-space indent; keep comments inline.

```yaml
releases:
  id: 00000
  rss: https://example.com/project/releases.atom
security:
  cpe:
    - vendor: example
      product: project
```

## Release tracking

- **`releases.id`** — numeric [release-monitoring.org](https://release-monitoring.org) (Anitya) id; the number in the project URL.
- **`releases.rss`** — Atom/RSS feed URL; use `~` if none exists.
- **`releases.ignore`** — regexes (prefer anchored `^`/`$`) to skip versions the repo doesn't track, with a comment.

Common feed patterns:
- **GitHub**: `https://github.com/<org>/<repo>/releases.atom` or `.../tags.atom`
- **GitLab / KDE Invent**: project URL + `/-/tags?format=atom`
- **PyPI**: `~` (rely on the Anitya id)
- **Freedesktop GitLab**: `https://gitlab.freedesktop.org/<path>//-/tags?format=atom`

## Security metadata

- **`security.cpe`** — list of NVD CPE `{vendor, product}` entries; add every applicable one.
- **`security.ignore`** — optional CVE ids/regexes to ignore.
- No CPE → `cpe: ~` with a dated comment. The `cpesearch <pkg>` helper finds related CPEs.

Keep the file in sync when upstream releases move/renamed.
