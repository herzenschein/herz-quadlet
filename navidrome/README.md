## Summary

Navidrome is a tag-based music streaming server based on [Subsonic](https://www.subsonic.org/pages/index.jsp).

Any client that can use Subsonic API should be able to use Navidrome.

It is not a remote-controlled music player. For that, use MPD.

* Main website: https://www.navidrome.org/
* Container docs:
  * https://www.navidrome.org/docs/installation/docker/
* See also:
  * [Jellyfin](../jellyfin)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user navidrome
```
