## Summary

Conduit is a [Matrix](https://matrix.org/) server that is written in Rust and uses an embedded NoSQL database called RocksDB.

It aims to be stable (which means slower updates) and provides administrative tools by default.

* Main website: https://conduit.rs/
* Container docs:
  * https://docs.conduit.rs/deploying/docker.html
* See also:
  * [Continuwuity](../continuwuity)
  * [Synapse](../synapse)
  * [Cinny](../cinny)
  * [Element](../element)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Just change the `CONDUIT_SERVER_NAME`, then start the server:

```bash
systemctl daemon-reload --user
systemctl start --user conduit
```

Refer to the [Synapse Reverse Proxying](../synapse/README.md#reverse-proxying)
instructions to manage your Conduit Server and your Homeserver with a reverse
proxy. It has the same behavior.

Note that unlike Synapse, which is closed for registrations by default and
requires you to run `podman exec` to generate an admin user, Conduit defaults
to open registrations and the first user to register in it becomes admin.
