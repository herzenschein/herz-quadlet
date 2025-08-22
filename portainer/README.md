## Summary

Portainer is a container manager.

It was originally designed for Docker, but later got Podman support.

* Main website: https://www.portainer.io/
* Container docs:
  * https://docs.portainer.io/start/install-ce/server/docker/linux

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user portainer
```
