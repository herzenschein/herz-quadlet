## Summary

Wallabag is a read-it-later service. It allows you to store webpages in the server for later reading.

* Main website: https://wallabag.org/
* Container docs:
  * https://doc.wallabag.org/admin/installation/installation/#installation-with-docker-or-docker-compose

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user wallabag
```
