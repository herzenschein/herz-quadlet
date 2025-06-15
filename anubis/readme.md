## Running instructions

Change the environment variables to point to where the service you want to protect is hosted.

You can change the `Network=` to point to an existing network where your reverse proxy is.

Then change the Caddy config (or other reverse proxy configuration you have) to follow usptream: https://anubis.techaro.lol/docs/admin/environments/caddy

```
# conf/Caddyfile

yourdomain.example.com {
  tls your@email.address

  reverse_proxy http://anubis:3000 {
        header_up X-Real-Ip {remote_host}
        header_up X-Http-Version {http.request.proto}
    }
}
```
