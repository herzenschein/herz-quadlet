## Summary

Forgejo is a web git forge that is a hard fork of [Gitea](https://gitea.com), which itself is a hard fork of [Gogs](https://gogs.io/).

It aims to be the first to provide [federation](https://codeberg.org/forgejo-contrib/federation/).

* Main website: https://forgejo.org/
* Container docs:
  * https://forgejo.org/docs/latest/admin/installation/docker/
* See also:
  * [Gitea](../gitea)
  * [Gitlab](../gitlab)
  * [Fossil](../fossil)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user forgejo
```
