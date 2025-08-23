## Summary

Web-check is a service that allows you to scan websites for known vulnerabilities and potential optimizations.

* Main website: https://web-check.xyz/
* Container docs:
  * https://web-check.xyz/self-hosted-setup

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user web-check
```
