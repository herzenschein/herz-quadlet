## Summary

Caddy is a web server and reverse proxy with automatic HTTPS. It can be used to make IP addresses accessible via HTTPS and a (sub)domain.

* Main website: https://caddyserver.com/
* Container links: https://hub.docker.com/_/caddy

## Running instructions

Copy this folder to `~/.config/containers/systemd/caddy/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user caddy
```

## Usage

To host a website using the contents of a specific folder, uncomment and edit the `/path/to/site` volume, add a folder inside it with the website files, then add an entry to the Caddyfile with something like:

```caddy
subdomain.domain.tld {
    root * /srv/websitecontents
    file_server
}
```

To host a simple FTP-like file server, simply use `file_server browse`:

```caddy
subdomain.domain.tld {
    root * /srv/websitecontents
    file_server browse
}
```

To host another container service, use `reverse_proxy` with the local server IP + the outbound port:

```caddy
subdomain.domain.tld {
    reverse_proxy serverIP:4533
    file_server
}
```

The server IP should be a `/32` IP as shown in `ip --brief address`.

If both containers belong to the same network (that is, both quadlets share the same `.network` file), then it is possible to leverage aardvark/netavark container DNS resolution and use the container name:

```caddy
subdomain.domain.tld {
    reverse_proxy navidrome:4533
    file_server
}
```

Note that if you use the server IP, you will need to use the outbound port and open it in the firewall; if you use internal container DNS resolution, you will need to use the inbound port and won't need to open a port in the firewall.

For example, if the container has the following `PublishPort`:

```systemd
PublishPort=5555:4533
```

The outbound port is `5555`, while the inbound port is `4533`.

If you intend to use the service locally in a LAN, you will need to open the outbound port in the firewall, even if you are using internal container DNS resolution. This way you can access a service like Navidrome locally in your LAN via IP, getting better speeds.

After setting the new entry in the Caddyfile, remember to create an A or AAAA DNS record in your DNS provider of choice. Then, restart Caddy:

```bash
systemctl daemon-reload --user
systemctl restart --user caddy
```

## Administrative reload

Instead of restarting the container itself, you may reload Caddy with no downtime using the admin port.

Uncomment `PublishPort=2019:2019` in the container, reload it once:

```bash
systemctl daemon-reload --user caddy
systemctl restart --user caddy
```

And from this point onwards, you may reload Caddy with the following command instead:

```bash
podman exec --workdir /etc/caddy caddy caddy reload
```

## Debugging

With `LogDriver=journald` set in the container, Caddy logs will automatically be available in the systemd journal. To access it, use:

```bash
journalctl --user-unit caddy
```

You may add the `--follow` flag to see live log updates or the `--pager-end` flag to see the end of the logs.

Caddy will provide additional debug output with the global [debug](https://caddyserver.com/docs/caddyfile/options#debug) setting in the Caddyfile:

```caddy
{
    debug
}
```

## Using plugins

Plugins can be added to Caddy by rebuilding the container, as documented in [Adding custom Caddy modules](https://hub.docker.com/_/caddy#adding-custom-caddy-modules).

For example, to build a plugin with Cloudflare DNS and Anubis, create a Containerfile with the following contents:

```container
FROM docker.io/caddy:builder AS builder

RUN xcaddy build \
    --with github.com/caddy-dns/cloudflare \
    --with github.com/daegalus/caddy-anubis

FROM docker.io/caddy:latest

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
```

Then build the container:

```bash
podman build --file caddy-with-cloudflare-and-anubis --tag caddy-with-cloudflare-and-anubis
```

Change the `Image=` section of the container:

```systemd
Image=caddy-with-cloudflare-and-anubis
```

And restart the container:

```bash
systemctl daemon-reload --user
systemctl restart --user caddy
```
