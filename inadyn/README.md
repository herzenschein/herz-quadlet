## Summary

Inadyn is an automated dynamic DNS client that lets you update DNS records automatically when your dynamic IP changes.

It does not allow to update only IPv6. For that use [ddns-updater](../ddns-updater).

* Main website: https://troglobit.com/projects/inadyn/
* Container docs:
  * https://github.com/troglobit/inadyn#docker
* See also:
  * [ddns-updater](../ddns-updater)
  * [Caddy](../caddy)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Download one of the example configuration files from the [upstream examples list](https://github.com/troglobit/inadyn/tree/master/examples):

```bash
wget https://raw.githubusercontent.com/troglobit/inadyn/refs/heads/master/examples/cloudflare-ipv6-only.conf --output-document inadyn.conf
```

Edit the file to your liking.

Then start the inadyn server:

```
systemctl daemon-reload --user
systemctl start --user inadyn
```
