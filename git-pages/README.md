## Summary

git-pages is a static website service that can be pushed to from a repository.

* Main website: https://git-pages.org/
* Container docs:
  * https://codeberg.org/git-pages/git-pages#deployment
* See also:
  * [Caddy](../caddy)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user answer
```

The service itself will not really do anything. The only thing it does is show static content if anything has been pushed to it.

The `/app/data` volume is not for website files, just persistent data.

To push to it, you will need to use [git-pages-cli](https://codeberg.org/git-pages/git-pages-cli),
a [Forgejo Action](https://codeberg.org/git-pages/action),
a custom CI config like Woodpecker that runs git-pages-cli,
or run curl commands.

The easiest setup works like this:

1. Run git-pages in your server
2. Point your reverse proxy to the git-pages service (I recommend [Caddy](../caddy))
3. In your Codeberg repository, create a Forgejo action for your website that builds it and point the `site:` to the desired domain or subdomain
4. Add a DNS record of type TXT to your DNS provider:
  * with the name `_git-pages-forge-allowlist` + `.domain.name`
  * with your allowed repo `"https://codeberg.org/username/reponame.git"`
5. Run the Action

This allows for the following workflow: push to git repo -> action calls git-pages-cli -> DNS TXT record authorizes push -> push to website domain.

Internally, the Forgejo Action just calls `git-pages-cli domain.name`, among other parameters. So if you're using Woodpecker CI or something else, just run the tool in the CI.

Naturally, `.domain.name` may be changed to `.subdomain.domain.name`, and these are just placeholders matching your specific website.

Make sure that the `site:` key in the action matches the DNS TXT record ***name***.

Note for Cloudflare users: you will need to perform this [workaround](https://codeberg.org/git-pages/git-pages-cli/issues/33) to be able to use the tool with Cloudflare's DNS proxy.
