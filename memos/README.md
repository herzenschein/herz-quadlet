## Summary

Memos is a mobile-friendly Markdown note-taking app designed for single or multiple users.

It comes with a tag system, memos link sharing, and an interface resembling microblogging platforms.

* Main website: https://www.usememos.com/
* Container docs:
  * https://www.usememos.com/docs/installation/docker
* See also:
  * [Etherpad](../etherpad)
  * [Rustpad](../rustpad)
  * [Silverbullet](../silverbullet)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user memos
```

The first logged-in user will be the admin.

If using it as a single user, log in, go to Settings -> System, and `Disallow user registration`.
