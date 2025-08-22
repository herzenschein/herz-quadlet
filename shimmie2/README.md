## Summary

Shimmie2 is a lightweight [Booru](https://en.wikipedia.org/wiki/Imageboard#Booru)-style image gallery with a focus on art.

* Main website: https://github.com/shish/shimmie2
* Container docs:
  * https://github.com/shish/shimmie2/wiki/Docker
* See also:
  * [Pigallery2](../pigallery2)
  * [Stash](../stash)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user shimmie2
```
