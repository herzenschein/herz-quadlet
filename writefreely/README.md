## Running instructions

See my other repo for a detailed explanation on how to set WriteFreely up:

https://github.com/herzenschein/herz-podman/tree/main/writefreely

Quickstart:

Generate the config file:

```
touch writefreely-config.ini
chmod 606 writefreely-config.ini
podman run --name writefreely --rm --interactive --tty --mount type=bind,source=./writefreely-config.ini,destination=/go/config.ini,rw,relabel=private docker.io/writeas/writefreely:latest config generate
```

Edit the container file to point to where the config.ini is.

Change the following entries in the writefreely-config.ini file:

```
bind = 0.0.0.0
username = same as MYSQL_USER in writefreely-db.container
password = same as MYSQL_PASSWORD in writefreely-db.container
database = same as MYSQL_DATABASE in writefreely-db.container
hostname = same as the container file, here writefreely-db
single_user = false
open_registration = true
```

Reload the systemd daemon:

```
systemctl daemon-reload --user
```

Start the WriteFreely container:

```
systemctl start --user writefreely-web
```

It will fail, while writefreely-db will keep running. Run the following:

```
podman run --network systemd-writefreelynet --name writefreely --rm --interactive --tty --mount type=bind,source=./writefreely-config.ini,destination=/go/config.ini,rw docker.io/writeas/writefreely:latest db init
```

The database is ready and WriteFreely should work now. Restart it:

```
systemctl restart --user writefreely-web
```
