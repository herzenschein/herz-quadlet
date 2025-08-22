## Summary

Jellyfin is a multimedia center that lets you view movies, shows, music and books.

It comes with features like SyncPlay that allow multiple users to watch the same media.

As it's a generic solution, it's not great for music. Use Navidrome for that.

* Main website: https://jellyfin.org/
* Container docs:
  * https://jellyfin.org/docs/general/installation/container
* See also:
  * [Navidrome](../navidrome)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user jellyfin
```
