## Summary

Redis is primarily a cache server and key/value store database
(not to be mistaken with databases like PostgreSQL and MariaDB).

It is used by other containers that connect to it directly via internal container networking,
so under normal circumstances you don't need to publish a port.

You will probably want one Redis instance per service.

* Main website: https://redis.io/
* Container docs:
  * https://hub.docker.com/_/redis/
* See also:
* Services that support Redis:

## Running instructions

Copy this folder to `~/.config/containers/systemd/`,
inside the same folder as one of your other services.

Change the container name to match something like `<service>-redis`,
for instance `nextcloud-redis`. This is useful to differentiate between multiple Redis instances.
You might also want to change the filename to the same naming scheme.

**Make sure it is in the same network as the service you are trying to run.**

Typically the service you're using it for provides a `REDIS_HOST` variable or similar.
Set it to the chosen container name.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user someservice-redis someservice
```
