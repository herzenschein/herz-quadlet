## Summary

Anubis is a Web AI Firewall Utility that protect your website from scraper bots.

* Main website: https://anubis.techaro.lol/
* Container docs:
  * https://anubis.techaro.lol/docs/admin/environments/docker-compose
  * https://github.com/daegalus/caddy-anubis
* See also:
  * [Caddy](../caddy)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Change the environment variables to point to where the service you want to protect is hosted.

You can change the `Network=` to point to an existing network where your reverse proxy is.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user anubis
```

Then change the Caddy config (or other reverse proxy configuration you have) to follow usptream: https://anubis.techaro.lol/docs/admin/environments/caddy

```
yourdomain.example.com {
  reverse_proxy http://anubis:3000 {
        header_up X-Real-Ip {remote_host}
        header_up X-Http-Version {http.request.proto}
    }
}
```

Alternatively, consider using [caddy-anubis](https://github.com/daegalus/caddy-anubis). This does not require setting an Anubis instance for each individual service.

You will need to create a custom Caddy container with the plugin following the [Docker documentation](https://hub.docker.com/_/caddy#adding-custom-caddy-modules), making sure to build for the right architecture.

After starting the new container, you can hide services behind Anubis with the following snippet:

```
yourdomain.example.com {
	reverse_proxy myservice:someport
	anubis
	request_header +X-Real-IP {remote_host}
	request_header +X-Forwarded-For {remote_host}
}
```
