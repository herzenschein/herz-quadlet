## Comparison between Quadlets and other solutions

It deprecates, but is not a direct alternative to,
[podman-generate-systemd](https://docs.podman.io/en/latest/markdown/podman-generate-systemd.1.html).
It is more of an alternative to `docker-compose.yml` or `compose.yml` files.
So instead of writing compose files following the
[Compose Specification](https://compose-spec.io/),
and instead of using something like
[docker compose](https://docs.docker.com/compose/) or
[podman compose](https://github.com/containers/podman-compose),
you write files in INI format, similarly to a systemd unit.

It is not a direct alternative to podman-generate-systemd because it does not
generate the new format using your existing compose files. You need to manually
create new files based on them. Thankfully, there is a tool called
[Podlet](https://github.com/k9withabone/podlet) that is able to generate
container files for you based on `podman run`, `podman compose` or a
`{docker-}compose.yml`.
