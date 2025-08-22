## Summary

Wekan is a kanban-style project management software.

* Main website: https://wekan.fi/
* Container docs:
  * https://github.com/wekan/wekan/blob/main/docker-compose.yml
* See also:
  * [Kanboard](../kanboard)
  * [Peppermint](../peppermint)
  * [Wekan](../wekan)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user wekan
```
