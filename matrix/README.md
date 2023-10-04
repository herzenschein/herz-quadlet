## Running instructions

This is a minimal example that just uses the default SQLite database.
This will change in the future when I figure out a more robust default.

```
systemctl daemon-reload --user
systemctl start --user synapse-generate
systemctl start --user synapse
podman exec --interactive --tty synapse register_new_matrix_user http://localhost:8008 -c /data/homeserver.yaml
```

Since the container does not come with `nano` or `vim`, the only way you may
be able to edit `/data/homeserver.yaml` would be to mount the folder directly
on your system.
