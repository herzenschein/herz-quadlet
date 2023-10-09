## Running instructions

```bash
systemctl daemon-reload --user
systemctl start --user mediawiki
```

Leave the `Volume=` mentioning `LocalSettings.php` commented.

Go to `http://localhost:8080` and follow the Wiki setup. When it asks for the
database host/server, use your IP (`localhost` or `127.0.0.1` won't work).

By the end of the procedure you will be able to download a generated
`LocalSettings.php` file, which you should put somewhere on your filesystem,
modify `mediawiki.container` to point to where the file is, and then remove the
`#` comment. Then:

```bash
systemctl daemon-reload --user
systemctl restart --user mediawiki
```
