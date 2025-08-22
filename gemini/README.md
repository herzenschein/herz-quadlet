## Summary

Gemini is a [Gemini Protocol](https://geminiquickst.art/)-using server for hosting websites starting with the `gemini://` URI instead of `https://`.

* Main website: https://github.com/a-h/gemini
* Container docs:
  * https://github.com/a-h/gemini#gemini-server-docker-image

## Running instructions

Copy this folder to `~/.config/containers/systemd/`.

Navigate to ~/.config/containers/systemd/gemini/certs, then run:

```bash
openssl ecparam -genkey -name secp384r1 -out server.key
openssl req -new -x509 -sha256 -key server.key -out server.crt -days 3650
```

You can instead use [certified](https://code.lag.net/robey/certified) or [gemcert](https://git.sr.ht/~solderpunk/gemcert) to generate the certificates if you want to.

If using OpenSSL, you can skip any undesirable options with a dot (`.`). Just make sure that the domain is correct. Next, change the DOMAIN in the container file to match.

Then run:

```bash
systemctl daemon-reload --user
systemctl start --user gemini
```

Because the certificate is self-signed and not verified by a Certificate Authority (CA), it will show a warning on most Gemini browsers by default.

This is generally fine and most Gemini browsers have a setting to disable this (as they encourage Trust-On-First-Use certificates, or TOFU) , but if that nags you you'll need to use [Let's Encrypt's Certbot](https://letsencrypt.org/) or [ZeroSSL](https://zerossl.com/) to get verified certificates and use those instead.

For development purposes you might want to try [making your own SSL CA](https://deliciousbrains.com/ssl-certificate-authority-for-local-https-development/).
