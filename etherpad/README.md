## Summary

Etherpad is a collaborative online editor for plain text / HTML files with a rich plugin ecosystem.

* Main website: https://etherpad.org/
* Container docs:
  * https://docs.etherpad.org/docker.html
  * https://github.com/ether/etherpad-lite#installation
* See also:
  * [Rustpad](../rustpad)
  * [Silverbullet](../silverbullet)
  * [Memos](../memos)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user etherpad
```
