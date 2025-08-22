## Summary

> [!WARNING]
> This quadlet was created before the upstream project started using podman quadlets.
> They recommend running it [rootful](https://wg-easy.github.io/wg-easy/latest/examples/tutorials/podman-nft/).
> I have used it rootless before and for most purposes it is fine, but sometimes if left inactive for too long the connection might be dropped.

wg-easy is a web frontend to manage [Wireguard VPN](https://www.wireguard.com/) configurations for multiple machines and users.

* Main website: https://github.com/wg-easy/wg-easy
* Container docs:
  * https://wg-easy.github.io/wg-easy/latest/examples/tutorials/podman-nft/

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Edit the file to your liking.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user wg-easy
```
