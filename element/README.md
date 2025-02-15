## Running instructions

First download the configuration file to the same directory as the cinny container file, then run it:

```bash
wget https://raw.githubusercontent.com/element-hq/element-web/refs/heads/develop/config.sample.json --output-document ~/.config/containers/systemd/element/config.json
systemctl daemon-reload --user
systemctl start --user element
```

You can also run it without any configuration file, in which case you can comment out the respective Volume.

A minimal, UNTESTED configuration for a single user instance would be like so:

```json
{
    "default_server_config": {
        "m.homeserver": {
            "base_url": "https://matrix.yourhomeserver.com",
            "server_name": "yourhomeserver.com"
        }
    },
    "disable_guests": true,
    "disable_custom_urls": true,
    "force_verification": true,
    "default_federate": false,
    "room_directory": {
        "servers": [
            "yourhomeserver.com",
            "matrix.org"
        ]
    }
}
```

Note that `base_url` needs to point to the (sub)domain where the Matrix server is actually hosted, not to the server name (used for user nicknames).
