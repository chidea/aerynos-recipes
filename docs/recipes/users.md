# System users

JSON User Records — see the [systemd User Record](https://systemd.io/USER_RECORD/) docs. System accounts are always **`locked`**; aerynOS ships user definitions **without** `privileged` or `signature` fields.

Add `pkg/gdm.user`:
```json
{
    "userName": "gdm",
    "realName": "GNOME Display Manager",
    "uid": 21,
    "gid": 21,
    "disposition": "system",
    "locked": true
}
```
`disposition` is always `system`; `homeDirectory` may be needed for some packages.

Install by username **and** by uid into `%(libdir)/userdb`:
```
%install_file %(pkgdir)/gdm.user %(installroot)%(libdir)/userdb/gdm.user
ln -s gdm.user %(installroot)%(libdir)/userdb/21.user
```
