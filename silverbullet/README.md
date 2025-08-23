## Summary

Silverbullet is a web-based, Markdown notetaking server designed for a single user.

It is simple to use, has the best Markdown QoL features I've seen in any Markdown editor (including web and native), has various complex features, and features a login system.

* Main website: https://silverbullet.md/
* Container docs:
  * https://silverbullet.md/Install/Docker
* See also:
  * [Etherpad](../etherpad)
  * [Rustpad](../rustpad)
  * [Memos](../memos)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user silverbullet
```
