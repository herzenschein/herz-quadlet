## Running instructions

Grab your IP using `ip --brief address` and then change gitlab.container to
point to it in `HostName=` and `external_url`. The container file includes
`127.0.0.1` simply as a placeholder that works for local testing.

Start the container:

```bash
podman pull docker.io/gitlab/gitlab-ce
systemctl daemon-reload --user
systemctl start --user gitlab
```

Because the Gitlab image has about 6 GiB in size, pulling the container image
directly via the quadlet is not advised, as it might exceed the default 5 min
systemd timeout by a large margin. Depending on your internet connection,
it can take from 12 to 20 minutes for the image to be pulled.

After that, once the service is started, Gitlab should take about 5 minutes
before it is fully set up. You will know that the service is fully set up when
you start seeing a lot of JSON text like `{"severity":"INFO", etc` in the logs
with `journalctl --follow --user-unit gitlab`.

Once you are able to see the webpage on
`http://yourIP:8888` and before 24 hours pass, run:

```bash
podman exec --interactive --tty gitlab grep 'Password:' /etc/gitlab/initial_root_password
```

Do not run the above command immediately after starting the service with
systemctl, otherwise you might get the error
`grep: /etc/gitlab/initial_root_password: No such file or directory`
because Gitlab has not created this file yet.
Wait at least 30 seconds before running it.

You should then be able to log in as the user `root` using the password gotten
via the above command.
Once logged in, you may visit the administrative area by going to
`yourIP:8888/admin`.

You cannot use `localhost` for `yourIP`, as that will fail container DNS
resolution, and you can use `127.0.0.1` for local testing, but it will fail
resolution later on when you try Gitlab Runner. If you are running this Quadlet
under a publicly exposed server, you should use the domain or subdomain name
for both `HostName=` and `external_url`.

When using a different published port, unlike other container services,
you may **NOT** do something like `8888:80`. You must use `8888:8888` and
necessarily set `external_url` to point to `yourIP:8888`, as mentioned in the
[Gitlab Documentation](https://docs.gitlab.com/ee/install/docker.html#expose-gitlab-on-different-ports).

The `8080` port is used internally by Gitlab, so it should not be used as the
published port, as it may cause internal connection issues.

If you ever see the error message `unable to dial unix socket` in the journalctl
logs for the container, it is because of a mismatch between the hostname and
the external URL.

Additional Omnibus configuration settings can be found in the
[gitlab.rb sample](https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/files/gitlab-config-template/gitlab.rb.template).
Multiple entries can be added using the `;` separator. For example:

```
Environment=GITLAB_OMNIBUS_CONFIG="external_url 'http://yourIP:8888'; gitlab_rails['gitlab_default_theme'] = 2;"
```

## Gitlab Runner and Continuous Integration

The Gitlab quadlet also comes with Gitlab Runner to enable CI builds by default.
When running rootless, its socket volume must point to your actual user socket,
which can be enabled via `systemctl enable --now --user podman.socket` and
whose path can be checked with `systemctl status --user podman.socket`.
The path should be `${XDG_RUNTIME_DIR}/podman/podman.sock`, in most cases
`/run/user/1000/podman/podman.sock`.

You may visit `http://yourIP:8888/admin/runners/` to then create a new runner.
Once the runner is created, you will be provided with a command like this:

```bash
gitlab-runner register --url http://yourIP:8888 --token sometoken
```

You can simply prepend the command with `podman exec gitlab-runner`, for example:

```bash
podman exec --interactive --tty gitlab-runner gitlab-runner register --url http://yourIP:8888 --token sometoken
```

Note that the first `gitlab-runner` is the container name, and the second is
the container entrypoint.
