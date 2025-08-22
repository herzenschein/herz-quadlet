## Summary

Peppermint is an issue management tracker.

* Main website: https://peppermint.sh/
* Container docs:
  * https://docs.peppermint.sh/docker
* See also:
  * [Focalboard](../focalboard)
  * [Kanboard](../kanboard)
  * [Wekan](../wekan)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Modify:

* POSTGRES_USER in peppermint-postgres.container
* POSTGRES_PASSWORD in peppermint-postgres.container
* DB_USERNAME in peppermint.container
* DB_PASSWORD in peppermint.container
* SECRET in peppermint.container

You can use something like `pwgen --secure 13 1` to generate a random secret and/or password.

Afterwards, run:

```bash
systemctl daemon-reload --user
systemctl start --user peppermint
```

Your Peppermint instance should now be running on localhost:3000.

The default user should be `admin@admin.com` and the default password should be `1234` (unless you modified it).
