This is just a file for me to make things easier when updating this repository.

These might be useful to you, too.

## Making large edits with Crudini

[Crudini](https://github.com/pixelb/crudini) is a command line tool that allows to edit ini files programmatically.

Here's a [fish](https://fishshell.com/) oneliner to update only containers that should start on boot:

```fish
for file in (find . -name "*.container" -not -name "*generate.container" -not -name "*build.container" -not -name "*init.container"); crudini --ini-options=nospace --set $file "Install" "WantedBy" "default.target"; end
```

I used this for commit 66dfab61a0235991cbeae905931aef9ed952f7d1.

The above adds:

```ini
[Install]
WantedBy=default.target
```

To all containers that do not have "generate", "build", or "init" in their name (in other words, containers that are supposed to start on boot).

If the entry already exists, it will simply change the existing value.

## Making a container start only after a device is mounted

The easiest way to do this is with
[x-systemd.automount](https://www.freedesktop.org/software/systemd/man/latest/systemd.mount.html#fstab) on a user directory.

In your `/etc/fstab`, create an entry that points to one of your user directories,
then add `x-systemd.automount` to the end of the comma-separated options. For example:

```bash
# uid and gid are included for exFAT support
/dev/sda2 /home/youruser/externalstorage auto rw,users,nofail,uid=1000,gid=1000,x-systemd.automount 0 0
```

To apply the settings immediately, run:

```bash
sudo systemctl daemon-reload
sudo systemctl restart remote-fs.target local-fs.target
```

This will make systemd automatically generate the following files: `home-youruser-externalstorage.mount` and `home-youruser-externalstorage.automount`.

To make the service load on boot and depend on the external storage mount,
add the following to the install section of your container file:

```ini
WantedBy=default.target home-youruser-externalstorage.mount
```

Done.
