## Summary

Apache Answer is a forum server designed for Q&A with gamification features.

Feels like a minimalistic Discourse.

* Main website: https://answer.apache.org/
* Container docs:
  * https://answer.apache.org/docs/installation?method=docker
* See also:
  * [Storyden](../storyden)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user answer
```
