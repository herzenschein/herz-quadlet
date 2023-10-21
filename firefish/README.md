## Running instructions

Edit [firefish.env](./firefish.env) and [default.yml](./firefish-config/default.yml)
so that the PostgreSQL information is safe and matching.

Make sure that the `host:` field of PostgreSQL and Redis in the default.yml file
matches their respective container names, NOT `localhost`. Then:

```bash
systemctl daemon-reload --user
systemctl start --user firefish
```

Visit `localhost:3000`. The first user to register will be admin.
