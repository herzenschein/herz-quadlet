## Running instructions

Start the container.

Once you are able to see the webpage on `http://127.0.0.1:8080` and before 24 hours pass, run:

```bash
podman exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
```

You should then be able to log in as the user `root` using the above password.
Once logged in, you may visit the administrative area by going to
`127.0.0.1:8888/admin`.

The provided container file runs on `127.0.0.1`, the localhost IP, by default.
For local testing this works well, as well as the host's IP as shown by
`ip --brief address`, but you cannot use `localhost`. If you are running
this Quadlet under a publicly exposed server, you should use the domain
or subdomain name for both `HostName=` and `external_url`.

When using a different published port, unlike other container services,
you may **NOT** do something like `8888:80`. You must use `8888:8888` and
necessarily set `external_url` to point to `ip:8888`, as mentioned in the
[Gitlab Documentation](https://docs.gitlab.com/ee/install/docker.html#expose-gitlab-on-different-ports).

The `8080` port is used internally by Gitlab, so it should not be used as the
published port, as it may cause internal connection issues.

If you ever see the error message `fail to dial unix socket` in the journalctl
logs for the container, it is because of a mismatch between the hostname and
the external URL.

Additional Omnibus configuration settings can be found in the
[gitlab.rb sample](https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/files/gitlab-config-template/gitlab.rb.template).
Multiple entries can be added using the ; separator. For example:

```
Environment=GITLAB_OMNIBUS_CONFIG="external_url '127.0.0.1:8888'; gitlab_rails['gitlab_default_theme'] = 2;"
```

## Gitlab Runner and Continuous Integration

The Gitlab quadlet also comes with Gitlab Runner to enable CI builds by default.
When running rootless, its socket volume must point to your actual user socket,
which can be enabled via `systemctl enable --now --user podman.socket` and
whose path can be checked with `systemctl status --user podman.socket`.
The path should be `${XDG_RUNTIME_DIR}/podman/podman.sock`, in most cases
`/run/user/1000/podman/podman.sock`.

You may visit `http://127.0.0.1:8888/admin/runners/` to then create a new runner.
Once the runner is created, you will be provided with a command like this:

```bash
gitlab-runner register --url http://127.0.0.1:8888 --token sometoken
```

You can simply prepend the command with `podman exec gitlab-runner`, for example:

```bash
podman exec --interactive --tty gitlab-runner gitlab-runner register --url http://127.0.0.1:8888 --token sometoken
```

Note that the first `gitlab-runner` is the container name, and the second is
the container entrypoint.
