## Summary

ddns-updater is an automated dynamic DNS client that lets you update DNS records automatically when your dynamic IP changes.

* Main website: https://github.com/qdm12/ddns-updater
* Container docs:
  * https://github.com/qdm12/ddns-updater/tree/master#container
  * https://github.com/qdm12/ddns-updater/tree/master/docs
* See also:
  * [inadyn](../inadyn)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Edit the `data/config.json` to your liking.

The current configuration example uses [Cloudflare](https://github.com/qdm12/ddns-updater/blob/master/docs/cloudflare.md) with IPv6. The Zone Identifier is in the "API Zone ID" at the bottom right of your domain dashboard. You can create an [API token](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/) with `Zone.Zone` and `Zone.DNS` as permissions.

Then start the ddns-updater server:

```
systemctl daemon-reload --user
systemctl start --user ddns-updater
```
