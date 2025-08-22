## Summary

synapse-admin is a web GUI to perform administrative actions for [Synapse](https://github.com/element-hq/synapse) and Synapse API compatible [Matrix](https://matrix.org/) servers.

Without this, you will need to use curl commands to administer your Synapse server.

* Main website: https://github.com/etkecc/synapse-admin
* Container docs:
  * https://github.com/etkecc/synapse-admin#steps-for-3
* See also:
  * [Synapse](../synapse)
  * [Conduit](../conduit)
  * [Continuwuity](../continuwuity)
  * [Cinny](../cinny)
  * [Element](../element)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user synapse-admin
```
