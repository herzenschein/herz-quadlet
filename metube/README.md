## Summary

Metube is a web interface for [youtube-dl](https://github.com/ytdl-org/youtube-dl) / [yt-dlp](https://github.com/yt-dlp/yt-dlp), allowing you to download videos from YouTube and other video-hosting websites.

* Main website: https://github.com/alexta69/metube
* Container docs:
  * https://github.com/alexta69/metube#run-using-docker

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user metube
```
