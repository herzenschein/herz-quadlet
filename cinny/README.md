## Running instructions

First download the configuration file to the same directory as the cinny container file, then run it:

```bash
wget https://raw.githubusercontent.com/cinnyapp/cinny/refs/heads/dev/config.json --output-document ~/.config/containers/systemd/cinny/config.json
systemctl daemon-reload --user
systemctl start --user cinny
``` 

If you want it to be completely limited to your own instance, you can use a config like this:

```json
{
    "allowCustomHomeservers": false,
    "homeserverList": [
        "yourhomeserver.com"
    ]
}
```

The registration button is always present, so if you don't want registration you should change the settings of your Matrix server.
