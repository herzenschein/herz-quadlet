## Summary

docker-mailserver is a no-database, production ready email server.

* Main website: https://docker-mailserver.github.io/docker-mailserver/latest/
* Container docs:
  * https://docker-mailserver.github.io/docker-mailserver/latest/usage/
  * https://docker-mailserver.github.io/docker-mailserver/latest/examples/tutorials/basic-installation/

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user dockermailserver
```
