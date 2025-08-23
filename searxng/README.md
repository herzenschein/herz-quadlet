## Summary

Searxng is a metasearch engine that aggregates results from Google, DuckDuckGo, Bing, and many other search engines.

* Main website: https://docs.searxng.org/
* Container docs:
  * https://docs.searxng.org/admin/installation-docker.html#installation-container

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user searxng
```
