## Summary

Stash is a lightweight image gallery with a focus on porn.

* Main website: https://github.com/stashapp/stash
* Container docs:
  * https://github.com/stashapp/stash/blob/develop/docker/production/README.md
* See also:
  * [Pigallery2](../pigallery2)
  * [Shimmie2](../shimmie2)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user stash
```
