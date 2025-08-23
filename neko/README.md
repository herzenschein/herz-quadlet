## Summary

Neko is a WebRTC virtual browser running in a full blown X server with Openbox and Pulseaudio.

This means you can use this to have a completely separate browser instance on a website that you can use for collaborative work and presentations.

You cannot do voice chat or share the screen of your host machine, but you may stream to it using RTMP.

* Main website: https://neko.m1k1o.net/
* Container docs:
  * https://neko.m1k1o.net/docs/v3/installation
* See also:
  * [Owncast](../owncast)

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Edit the file to your liking.

If running it locally, set `NEKO_NAT1TO1` to your host IP.
If the instance will be public, remove this environment variable.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user neko
```

Click on "Request Controls" at the bottom to control the browser window.
