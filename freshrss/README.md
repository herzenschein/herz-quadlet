## Summary

Freshrss is an RSS feed aggregator.

* Main website: https://freshrss.org/index.html
* Container docs:
  * https://github.com/FreshRSS/FreshRSS/tree/edge/Docker
* See also:
  * [Wallabag](../wallabag)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user freshrss
```
