## Summary

Gitea is a web git forge that is a hard fork of [Gogs](https://gogs.io/).

* Main website: https://gitea.com
* Container docs:
  * https://docs.gitea.com/installation/install-with-docker-rootless
  * https://docs.gitea.com/installation/install-with-docker
* See also:
  * [Forgejo](../forgejo)
  * [Gitlab](../gitlab)
  * [Fossil](../fossil)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user gitea
```
