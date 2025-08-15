## Summary

Continuwuity is a [Matrix](https://matrix.org/) homeserver.
It can be used to create your own Matrix community or to set up a single-user server.

It is a fork of the now dead [Conduwuit](https://conduwuit.puppyirl.gay/), which itself was a fork of [Conduit](https://conduit.rs/).

Main website: https://continuwuity.org

## Running instructions

Change the `CONTINUWUITY_SERVER_NAME` to the actual subdomain or domain you'll be using for Conduwuit.

Change `CONTINUWUITY_REGISTRATION_TOKEN` to a random key. You can do that with something like `pwgen --secure 13 1`.

Then:

```bash
systemctl daemon-reload --user
systemctl start --user continuwuity
```

After this, use a Matrix client that allows for registration to a homeserver, like Cinny or Element. Conduwuit does not allow creating new accounts via commands like Synapse does. The token you used for the quadlet will be necessary to register the new account, preventing registration spam.

The first account used to register will be the new admin account.

If you are going to use Continuwuity as a single-user server, you may then set `CONTINUWUITY_ALLOW_REGISTRATION` to `false` and then run:

```bash
systemctl daemon-reload --user
systemctl restart --user continuwuity
```

Lastly, uncomment `CONTINUWUITY_ALLOW_FEDERATION=true` and rerun the above commands to get your server to federate.

Refer to the [Synapse Reverse Proxying](../synapse/README.md#reverse-proxying)
instructions to manage your Conduwuit Server and your Homeserver with a reverse
proxy. It has the same behavior.
