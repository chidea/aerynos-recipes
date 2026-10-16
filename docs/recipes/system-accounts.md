# System accounts

aerynOS is stateless: packages/triggers **cannot** mutate `/etc/passwd` and co. Instead it integrates `nss-systemd` and `userdb`.

Benefit: no direct system-file mutation, and (unlike `sysusers`) removing a package removes its system user/group definitions.

See [Groups](groups.md) and [Users](users.md) for the record formats.
