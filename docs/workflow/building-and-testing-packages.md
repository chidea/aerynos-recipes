# Building and testing packages

Build a recipe locally and test it against the local repo. Keep the local repo disabled (or free of conflicting packages) before building.

```
just build                              # build the recipe
just mv-local                           # move the .stone into the local repo
sudo moss repo enable local             # if the local repo isn't enabled
sudo moss sync -u                       # refresh the local index
sudo moss install "package name"        # install (or it offers to update)
```

After testing, submit a PR (see *Submitting a PR*).
