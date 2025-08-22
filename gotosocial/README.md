## Summary

GoToSocial is an [ActivityPub](https://activitypub.rocks/) social network server to access the [Fediverse](https://en.wikipedia.org/wiki/Fediverse) for small or single user instances on low powered devices.

* Main website: https://gotosocial.org/
* Container docs:
  * https://docs.gotosocial.org/en/latest/getting_started/installation/container/
* See also:
  * [Owncast](../owncast)
  * [PeerTube](../peertube)
  * [Writefreely](../writefreely)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user gotosocial
```

The instance will be running in localhost:8080.

After that, you'll need to create the first admin account:

```bash
podman exec -it gotosocial /gotosocial/gotosocial admin account create \
    --username some_username \
    --email someone@example.org \
    --password 'some_very_good_password'
```

You can generate a random password using `pwgen --secure 13 1`.
