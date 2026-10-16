# Upstreams

Sources for a build. Most packages build from an upstream tarball; local assets are possible.

## Plain sources

A tarball with a URI and its **SHA256**:

```yaml
upstreams:
  - uri: <sha256>
```
or
```yaml
upstreams:
  - uri:
    hash: <sha256>
```

| Key       | Type      | Description                                  |
| --------- | --------- | -------------------------------------------- |
| hash      | `string`  | SHA256 of the upstream source                 |
| stripdirs | `string`  | Dirs to strip from the archive root           |
| unpack    | `boolean` | Whether to auto-unpack                        |
| unpackdir | `string`  | Force a different unpack directory            |

## Git sources

Provide a `tag` or `ref`. **Branch names are forbidden** (they mutate and break builds); use a full git ref.

```yaml
upstreams:
  - git|uri: <ref>
```

| Key       | Type     | Description                    |
| --------- | -------- | ------------------------------ |
| ref       | `string` | git ref                          |
| unpackdir | `string` | Override clone target directory  |
