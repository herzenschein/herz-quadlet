## Running instructions

First, download one of the example configuration files from the [upstream examples list](https://github.com/troglobit/inadyn/tree/master/examples):

```bash
wget https://raw.githubusercontent.com/troglobit/inadyn/refs/heads/master/examples/cloudflare-ipv6-only.conf --output-document inadyn.conf
```

Edit the file to your liking.

Then start the inadyn server:

```
systemctl daemon-reload --user
systemctl start --user inadyn
```
