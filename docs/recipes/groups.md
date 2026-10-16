# System groups

JSON Group Records — see the [systemd Group Record](https://systemd.io/GROUP_RECORD/) docs for all fields.

Add `pkg/gdm.group`:
```json
{
    "groupName": "gdm",
    "gid": 21,
    "disposition": "system"
}
```
Minimum fields; `disposition` is always `system`.

Install by group name **and** by gid into `%(libdir)/userdb`:
```
%install_file %(pkgdir)/gdm.group %(installroot)%(libdir)/userdb/gdm.group
ln -s gdm.group %(installroot)%(libdir)/userdb/21.group
```
