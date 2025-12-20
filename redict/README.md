## Summary

Redict is a drop-in replacement for Redis, primarily a cache server and key/value store database
(not to be mistaken with databases like PostgreSQL and MariaDB).

Redict is LGPL, while Redis has changed their licenses multiple times.
Where Redis is used, this can be used instead.

It is used by other containers that connect to it directly via internal container networking,
so under normal circumstances you don't need to publish a port.

You will probably want one Redict instance per service.

* Main website: https://redict.io/
* Container docs:
  * https://redict.io/docs/install/containers/
* See also:
  * [redis](../redis)
* Services that support Redis (and consequently also Redict):
  * [answer](../answer)
  * [caddy](../caddy)
  * [docker-mailserver](../dockermailserver)
  * [etherpad](../etherpad)
  * [forgejo](../forgejo)
  * [gitea](../gitea)
  * [gitlab](../gitlab)
  * [linkstack](../linkstack)
  * [mediawiki](../mediawiki)
  * [nextcloud](../nextcloud)
  * [nitter](../nitter)
  * [onlyoffice](../onlyoffice)
  * [peertube](../peertube)
  * [shimmie2](../shimmie2)
  * [storyden](../storyden)
  * [synapse](../synapse)
  * [wallabag](../wallabag)
  * [weblate](../weblate)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`,
inside the same folder as one of your other services.

Change the container name to match something like `<service>-redict`,
for instance `nextcloud-redict`. This is useful to differentiate between multiple Redict instances.
You might also want to change the filename to the same naming scheme.

**Make sure it is in the same network as the service you are trying to run.**

Typically the service you're using it for provides a `REDIS_HOST` variable or similar.
Set it to the chosen container name.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user someservice-redis someservice
```
