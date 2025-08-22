## Summary

OnlyOffice is a collaborative web office suite.

Before using it, you will need to integrate it to a [third party connector](https://www.onlyoffice.com/all-connectors.aspx) like [Nextcloud](https://nextcloud.com/).

* Main website: https://www.onlyoffice.com/
* Container docs:
  * https://helpcenter.onlyoffice.com/docs/installation/docs-community-install-docker.aspx
* See also:
  * [Nextcloud](../nextcloud)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user onlyoffice
```
