## Summary

Librespeed is a lightweight JS-only Speed Test.

* Main website: https://librespeed.org/
* Container docs:
  * https://github.com/librespeed/speedtest/blob/master/doc_docker.md

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user librespeed
```
