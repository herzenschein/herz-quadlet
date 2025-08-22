## Summary

Pigallery2 is a lightweight image gallery with a focus on photos.

Image descriptions can be added using the [IPTC Caption tag](https://www.iptc.org/std/photometadata/documentation/userguide/#_descriptioncaption).

* Main website: https://bpatrik.github.io/pigallery2/
* Container docs:
  * https://github.com/bpatrik/pigallery2/blob/master/docker/README.md
* See also:
  * [Shimmie2](../shimmie2)
  * [Stash](../stash)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user pigallery2
```

The default user and password is `admin`.

Log in with it, create a new user with Admin role, then delete the default admin account.
