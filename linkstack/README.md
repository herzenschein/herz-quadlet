## Summary

LinkStack is a managed link page for profile bios.

It counts as a selfhosted alternative to things like [LinkTree](https://linktr.ee/) and [Carrd](https://carrd.co/).

* Main website: https://linkstack.org/
* Container docs:
  * https://linkstack.org/docker/
  * https://github.com/linkstackorg/linkstack-docker

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user linkstack
```
