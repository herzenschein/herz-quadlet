## Summary

Kanboard is a kanban-style project management software.

* Main website: https://kanboard.org/
* Container docs:
  * https://docs.kanboard.org/v1/admin/docker/
* See also:
  * [Focalboard](../focalboard)
  * [Peppermint](../peppermint)
  * [Wekan](../wekan)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user kanboard
```
