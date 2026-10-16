# Submitting a PR

## One commit per package

Same rule as *Using git*. Commit creation is covered there.

## Push and open the PR

```
git push -u origin HEAD
```
Open a PR from your fork against `aerynOS/recipes`. GitHub has no template picker — use the **Preview** tab in the description to select the right template (new recipe / update / bug fix) and fill it in.

## PR / commit title format

- `name: Add at v<version>`
- `name: Update to v<version>`
- `name: Fix <...>`
- `[NFC] name: <no functional change>`

## Commit message format

- Imperative short summary
- Key changes (sentences or bullets)
- Changelog link (if the update spans several versions, list them ascending, newest last)
- **Test plan** proving it works locally
- `Resolves aerynos/recipes#<issue>` if it closes an issue

You take ownership of the quality of what you submit.
