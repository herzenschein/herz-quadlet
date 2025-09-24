## Summary

> [!WARNING]
> This quadlet was created before the upstream project started using podman quadlets.
> It was modified to follow the upstream recommendations, which uses more systemd features and is way more complex than my opinionated setup.
> Please refer to the upstream documentation instead.

docker-mailserver is a no-database, production ready email server.

* Main website: https://docker-mailserver.github.io/docker-mailserver/latest/
* Container docs:
  * https://docker-mailserver.github.io/docker-mailserver/latest/config/advanced/podman/#rootless-quadlet
  * https://docker-mailserver.github.io/docker-mailserver/latest/usage/
  * https://docker-mailserver.github.io/docker-mailserver/latest/examples/tutorials/basic-installation/

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Edit the file to your liking.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user dockermailserver
```
