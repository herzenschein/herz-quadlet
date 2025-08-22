## Summary

WriteFreely is an [ActivityPub](https://activitypub.rocks/) blog writing platform that is federated across the [Fediverse](https://en.wikipedia.org/wiki/Fediverse) designed for single or multiple users.

* Main website: https://writefreely.org/
* Container docs:
  * https://docs.kanboard.org/v1/admin/docker/
* See also:
  * [GoToSocial](../gotosocial)
  * [PeerTube](../peertube)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Create a `config.ini` file somewhere on your machine, then edit
writefreely-generate, writefreely-web and writefreely-dbinit to point to
where the config.ini is. Then, generate the config file:

```
systemctl daemon-reload --user
systemctl start --user writefreely-generate
```

Change the following entries in the writefreely-config.ini file:

```
bind = 0.0.0.0
username = same as MYSQL_USER in writefreely-db.container
password = same as MYSQL_PASSWORD in writefreely-db.container
database = same as MYSQL_DATABASE in writefreely-db.container
hostname = same as the container file, here writefreely-db
single_user = false
open_registration = true
```

Start the WriteFreely container:

```
systemctl start --user writefreely-web
```

It will fail, while writefreely-db will keep running. Initialize the database:

```
systemctl start --user writefreely-dbinit
```

The database is ready and WriteFreely should work now. Restart it:

```
systemctl restart --user writefreely-web
```
