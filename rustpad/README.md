## Summary

Rustpad is a collaborative web text editor with a focus on code.

* Main website: https://rustpad.io
* Container docs:
  * https://github.com/ekzhang/rustpad#deployment
* See also:
  * [Etherpad](../etherpad)
  * [Silverbullet](../silverbullet)
  * [Memos](../memos)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user rustpad
```
