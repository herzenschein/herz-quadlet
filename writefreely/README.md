## Running instructions

See my other repo for a detailed explanation on how to set WriteFreely up:

https://github.com/herzenschein/herz-podman/tree/main/writefreely

Quickstart:

Create a `config.ini` file somewhere on your machine, then edit
writefreely-generate, writefreely-web and writefreely-dbinit to point to
where the config.ini is. Then, generate the config file:

```
systemctl daemon-reload --user
systemctl start --user writefreely-generate
```

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

Start the WriteFreely container:

```
systemctl start --user writefreely-web
```

It will fail, while writefreely-db will keep running. Initialize the database:

```
systemctl start --user writefreely-dbinit
```

The database is ready and WriteFreely should work now. Restart it:

```
systemctl restart --user writefreely-web
```
