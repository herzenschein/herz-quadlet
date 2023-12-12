## Running instructions

Edit the `HostName=` in `peertube.container` to match `PEERTUBE_WEBSERVER_HOSTNAME=` in the env file.
By default localhost should work for local testing, but you should instead use your IP or domain.

Then edit the rest of the env file. Note that `PEERTUBE_REDIS_HOSTNAME=` should match the container name.

Afterwards, run:

```bash
systemctl daemon-reload --user
systemctl start --user peertube
```

Your Peertube instance should now be running at localhost:9000.

The default administrative user is `root`, and for the password, run:

```bash
journalctl --user-unit peertube --grep "User password"
```

This Quadlet does not yet include a functional Postfix configuration.
