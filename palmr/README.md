## Summary

Palmr is a simple server for file upload and sharing.

* Main website: https://palmr.kyantech.com.br/
* Container docs:
  * https://palmr.kyantech.com.br/docs/3.2-beta/quick-start#deployment-options
* See also:
  * [DumbDrop](../dumbdrop)
  * [Nextcloud](../nextcloud)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

If you uncomment `DISABLE_FILESYSTEM_ENCRYPTION=false`, then Palmr will use encryption to store your data. In this case, `ENCRYPTION_KEY` must also be enabled and set with a suitable 32-char key **minimum**. This can be generated with something like `pwgen --secure 40 1`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user palmr
```
