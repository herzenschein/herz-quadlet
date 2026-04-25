## Summary

copyparty is a file server with resumable uploads/downloads that works on everything.

* Main website: https://github.com/9001/copyparty
* Container docs:
  * https://github.com/9001/copyparty/tree/hovudstraum/scripts/docker
* See also:
  * [Nextcloud](../nextcloud)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Modify the `/w` volume to the folder where the cloud data will be stored.
You may optionally add more volumes inside or outside the default shared folder `/w`.

Modify the default accounts in `copyparty-config/copyparty.conf`.
You may use `pwgen --secure 13 1` to generate a secure password for each account.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user copyparty
```

Logging in works by typing only the password of a valid user.

Remember to map the volumes correctly, otherwise copyparty might start in memory mode
and you will become unable to get proper write access and be unable to upload.
