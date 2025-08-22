## Summary

Vaultwarden is a password manager that is compatible with [Bitwarden](https://bitwarden.com/) clients.

* Main website: https://github.com/dani-garcia/vaultwarden
* Container docs:
  * https://github.com/dani-garcia/vaultwarden#usage

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user vaultwarden
```
