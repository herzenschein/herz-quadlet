## Summary

Owncast is a [Twitch](https://www.twitch.tv/)-like video streaming service designed for a single user that is federated across the [Fediverse](https://en.wikipedia.org/wiki/Fediverse).

* Main website: https://owncast.online/
* Container docs:
  * https://owncast.online/quickstart/container/
* See also:
  * [PeerTube](../peertube)
  * [GoToSocial](../gotosocial)
  * [Writefreely](../writefreely)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user owncast
```
