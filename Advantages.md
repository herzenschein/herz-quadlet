## Advantages of using Quadlets

* Podman Pods require you to define ports and whatnot at Pod creation time,
which can be impractical if you need to change your setup in the future. With
Quadlets, you alter the container file, reload the daemon and restart the service.

* Using podman-generate-systemd always generates complex and hard-to-read
systemd service files. Quadlets do not have this issue.

* Using podman-generate-systemd together with podman-compose is not very robust:
after making drastic changes to a compose file and recreating the containers/
pods, the generated services will lose their ability to manage those containers.
With Quadlets, container files have an inherent connection to their respective
systemd services.

* Extending a service is as simple as adding or removing a few lines and
restarting the daemon and service.

* Systemd features are available to container files. Things like `OnFailure=`
to trigger another service if the current one fails, `ExecStopPost=` to run an
arbitrary command if the current service fails, container logs are stored in
[journald](https://www.freedesktop.org/software/systemd/man/systemd-journald.service.html)
and are accessible via
[journalctl](https://www.freedesktop.org/software/systemd/man/journalctl.html),
containers can be managed together with
[systemd timers](https://www.freedesktop.org/software/systemd/man/systemd.timer.html),
etc. This allows for capabilities that are not possible or are difficult to do
with compose files.

* Because all processes related to the container are under the same service
cgroup, systemd is able to manage it entirely, and changing the respective
cgroup affects the entire container.

* Because the containers are internally run with podman, you can still manage
them with podman as you wish. This is useful, for instance, if the container
requires a restart after some internal modification to the container (like
MediaWiki), or if it needs changes to its matching database (like WriteFreely).
