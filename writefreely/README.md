## Running instructions

See my other repo for a detailed explanation on how to set WriteFreely up:

https://github.com/herzenschein/herz-podman/tree/main/writefreely

Quickstart:

Generate the config file:

```
podman run --name writefreely --rm --interactive --tty --mount type=bind,source=./writefreely-config.ini,destination=/go/config.ini,rw,relabel=private docker.io/writeas/writefreely:latest config generate
```

Edit the container file to point to where the config.ini is.

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
podman run --network writefreely --name writefreely --rm --interactive --tty --mount type=bind,source=./writefreely-config.ini,destination=/go/config.ini,rw docker.io/writeas/writefreely:latest db init
```

The database is ready and WriteFreely should work now. Restart it:

```
systemctl restart --user writefreely-web
```
