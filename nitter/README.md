## Summary

Nitter is a private web front-end to view [Twitter/X](https://x.com/).

* Main website: https://github.com/zedeus/nitter
* Container docs:
  * https://github.com/zedeus/nitter#docker

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user nitter
```
