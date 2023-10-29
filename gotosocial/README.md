== Running instructions ==

```bash
systemctl daemon-reload --user
systemctl start --user gotosocial
```

The instance will be running in localhost:8080.

After that, you'll need to create the first admin account:

```bash
podman exec -it gotosocial /gotosocial/gotosocial admin account create \
    --username some_username \
    --email someone@example.org \
    --password 'some_very_good_password'
```
