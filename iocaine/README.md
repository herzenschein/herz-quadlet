## Summary

iocaine blocks and provides garbage output to LLM scrapers.

* Main website: https://iocaine.madhouse-project.org/
* Container docs:
  * https://iocaine.madhouse-project.org/documentation/3/getting-started/containers/
* See also:
  * [anubis](../anubis)
  * [caddy](../caddy)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user iocaine
```

In your Caddy config, you will need to add something like the following:

```caddy
blog.example.com {
  @read method GET HEAD
  reverse_proxy @read 127.0.0.1:42069 {
    @fallback status 421
    handle_response @fallback
  }
  root /var/www/blog.example.com
  file_server
}
```

Then reload Caddy.
