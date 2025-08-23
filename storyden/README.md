## Summary

Storyden is a forum software with knowledge base features designed for simplicity.

Feels like a mix of Reddit with Discourse.

* Main website: https://www.storyden.org/
* Container docs:
  * https://www.storyden.org/docs/introduction/vps/docker
* See also:
  * [Apache Answer](../answer)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

If storyden will be public, edit `PUBLIC_WEB_ADDRESS` and `PUBLIC_API_ADDRESS`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user storyden
```
