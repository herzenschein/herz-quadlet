## Summary

Hugo is a static website generator.

* Main website: https://gohugo.io/
* Container docs:
  * https://github.com/klakegg/docker-hugo#using-image (3rd party)

## Running instructions

There are two ways to use this container: with `hugo-build` to generate the public website files, or with `hugo-server` to run the [development server mode](https://gohugo.io/commands/hugo_server/) (not recommended by upstream).

Copy this folder to `~/.config/containers/systemd/`.

Change the /path/to/website volume to point to your Hugo website *sources*.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user hugo-build
# OR
systemctl start --user hugo-server
```

If using hugo-build, the generated files will be available in a `public/` folder inside your Hugo website sources. You may then use something like Caddy to publish the generated website.

If using hugo-server, note that the specified port will automatically change if the default `1313` port is already in use.
