# Using git

## One commit per package

Each recipe add/change is its own commit. Don't combine packages; don't leave multiple commits for one package. A PR may hold several commits when a package plus its new dependencies are added — each dependency is its own commit.

## Git hooks (from `just init`)

- `prepare-commit-msg` — prefills the commit title from the staged recipe:
  - `name: Add at v<version>` — first release
  - `name: Update to v<version>` — version changed
  - `[NFC] name:` — `stone.yaml` changed but not rebuilt (no new manifest)
  - `name:` — other recipe changes (fix, etc.)
- `pre-commit` — lints staged files; common packaging errors reject the commit.

Commit **from the recipe dir** so the hooks see the recipe: `chpkg nano && git commit`.

## Committing

```
chpkg nano
git status
git diff
git add stone.yaml manifest.x86_64.bin manifest.x86_64.jsonc monitoring.yaml
git add pkg/        # if patches / extra files
git commit
```

## Amending an existing commit

```
chpkg nano
fixup-recipe-commit     # stages + fixups + autosquashes the last commit for this dir
```
After pushing: `git push --force-with-lease`.

## Common commands

`gotoaosrepo`, `chpkg <name>`, `git status`, `git diff`, `git diff --staged`, `git log --oneline`, `git log -1 -- .`, `git branch -a`, `git push -u origin HEAD`, `git push --force-with-lease`, `git gone`.

## Review feedback

Stay on the PR branch and amend the affected package's existing commit — never add a second commit for the same package.
