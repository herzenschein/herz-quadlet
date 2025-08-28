## Advantages of using Quadlets

Quadlets have INI-like syntax, as they are based on
[systemd units](https://www.freedesktop.org/software/systemd/man/latest/systemd.unit.html).
This is much simpler to reason about than YAML, used for podman-compose and docker-compose.

---

Extending a service is as simple as adding or removing a few lines and
restarting the daemon and service.

---

Because the containers are internally run with podman, you can still manage
them with podman as you wish. This is useful, for instance, if the container
requires a restart after some internal modification to the container (like
MediaWiki), or if it needs changes to its matching database (like WriteFreely).

---

Podman provides [Pods](https://docs.podman.io/en/latest/markdown/podman-pod.1.html),
which allow to bundle groups of containers and, in addition to that,
automatically insert them into the same network.
Podman Pods require you to define ports and whatnot at Pod creation time,
which can be impractical if you need to change your setup in the future.
With Quadlets, you alter the container file, reload the daemon and restart the service.

---

The now deprecated [podman-generate-systemd](https://docs.podman.io/en/latest/markdown/podman-generate-systemd.1.html)
always generates complex and hard-to-read systemd service files.
Quadlets do not have this issue.

---

Using [podman-generate-systemd](https://docs.podman.io/en/latest/markdown/podman-generate-systemd.1.html)
together with [podman-compose](https://github.com/containers/podman-compose) is not very robust:
after making drastic changes to a compose file and recreating the containers/
pods, the generated services will lose their ability to manage those containers.
With Quadlets, container files have an inherent connection to their respective
systemd services.

---

Systemd features are available to container files. Things like `OnFailure=`
to trigger another service if the current one fails, `ExecStopPost=` to run an
arbitrary command if the current service fails, container logs are stored in
[journald](https://www.freedesktop.org/software/systemd/man/systemd-journald.service.html)
and are accessible via
[journalctl](https://www.freedesktop.org/software/systemd/man/journalctl.html),
containers can be managed together with
[systemd timers](https://www.freedesktop.org/software/systemd/man/systemd.timer.html),
etc. This allows for capabilities that are not possible or are difficult to do
with compose files. You can find more about this in the [How To](HowTo.md).

---

Because all processes related to the container are under the same service
cgroup, systemd is able to manage it entirely, and changing the respective
cgroup affects the entire container.
