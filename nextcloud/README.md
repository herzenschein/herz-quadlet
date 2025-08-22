## Summary

Nextcloud is a full cloud suite that originated as a fork of [Owncloud](https://owncloud.com/).

This is not Nextcloud All-In-One (AIO), as AIO uses Docker-specific API and is thus difficult to port to podman.

This is the much leaner community-maintained Nextcloud that does not come with plugins or extras. You will need to install plugins manually or selfhost additional services like [OnlyOffice](https://www.onlyoffice.com/) or [Collabora](https://www.collaboraonline.com/) if you want to use them.

* Main website: https://nextcloud.com/
* Container docs:
  * https://hub.docker.com/_/nextcloud/
* See also:
  * [OnlyOffice](../onlyoffice)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user nextcloud
```
