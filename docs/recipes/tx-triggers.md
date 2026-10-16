# Transaction triggers

`tx` triggers run after the new filesystem transaction is blitted to disk and just before the new `/usr` is activated. They run in a specialized container with **read-write** access to the new `/usr` and **read-only** access to `/etc`.

Install them in `/usr/share/moss/triggers/tx.d/*.yaml`.

## Sample trigger

Runs `depmod -a <version>` when files land in `/usr/lib/modules/<version>/` (identical expanded commands collapse to one run):

```yaml
name: depmod
description: |
    Update kernel module dependencies
handlers:
  depmod:
    run: /usr/sbin/depmod
    args: ["-a", "$(version)"]
paths:
  "/usr/lib/modules/(version:*)/*":
    handlers:
      - depmod
    type: directory
```

Install in a recipe:
```
%install_file %(pkgdir)/trigger.yaml %(installroot)/usr/share/moss/triggers/tx.d/gdk_pixbuf.yaml
```
