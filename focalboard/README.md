## Summary

> [!NOTE]
> This project has been marked as "unmaintained" upstream, though it works fine. If the project's status is not changed in the future, this folder will be removed.

Focalboard is a project management web dashboard that is highly visual with tons of emojis and colors.

* Main website: https://www.focalboard.com/
* Container docs:
  * https://github.com/mattermost-community/focalboard#building-and-running-standalone-desktop-apps
* See also:
  * [Kanboard](../kanboard)
  * [Peppermint](../peppermint)
  * [Wekan](../wekan)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user focalboard
```
