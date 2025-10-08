## Summary

Weblate is a selfhosted [Computer Assisted Translation (CAT) tool](https://en.wikipedia.org/wiki/Computer-assisted_translation) for translating software (localization), especially following the [GNU gettext](https://www.gnu.org/software/gettext/) standard files.

* Main website: https://weblate.org/
* Container docs:
  * https://docs.weblate.org/en/latest/admin/install/docker.html

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Change the `WEBLATE_SITE_DOMAIN` to the actual subdomain or domain you'll be using for Conduwuit.

Edit the non-host related database variables to your liking.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user weblate
```

If you haven't set `WEBLATE_ADMIN_PASSWORD`, the default `admin` user password will show up in the logs, which you can check with:

```bash
journalctl --user-unit weblate
```
