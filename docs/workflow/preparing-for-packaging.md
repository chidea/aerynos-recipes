# Preparing for packaging

Steps to run before creating or updating a recipe.

## Update the recipes clone

Ensure the volatile repo is enabled and the system is current:
```
sudo moss repo enable volatile
sudo moss sync -u
gotoaosrepo
git switch main
gh repo sync yourusername/yourfork -b main
git pull
```

## Switch to a new branch

Isolate packaging work on a branch so it can be rebased independently of `main`:
```
git checkout -b "branch-name"
```
Convention: `add-<package>` for new packages, `update-<package>` for updates. Check branches with `git branch -a`.

aerynOS requires **one commit per package**.
