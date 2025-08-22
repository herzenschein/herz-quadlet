## Summary

Fossil is a distributed SCM.

It is an extremely tiny all-in-one solution that provides distributed version control like git, as well as wiki, chat, forum, website, and other project management features.

It provides conveniences like [autosync](https://fossil-scm.org/home/doc/trunk/www/concepts.wiki#workflow), so a pull automatically occurs when you run `update` and a push automatically occurs after you `commit`, unlike git.

* Main website: https://fossil-scm.org/
* Container docs:
  * https://hub.docker.com/r/nijtmans/fossil (3rd party)
* See also:
  * [Forgejo](../forgejo)
  * [Gitea](../gitea)
  * [Gitlab](../gitlab)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user fossil
```

To log in, use `admin` as the username. Use `journalctl --user-unit fossil` to retrieve the default generated password for admin.
