# herz-quadlet
A repository where I store my podman quadlets.

## What are Quadlets?

Quadlets are a new way to manage containers. They consist of `.container` files.

They were originally a [separate project](https://github.com/containers/quadlet)
which has now been merged into [Podman](https://github.com/containers/podman).

By writing an INI file with the `.container` extension (as well as `.network`,
`.volume` and `.kube` if needed) and storing it in a certain directory,
[systemd](https://systemd.io/) is able to automatically generate a systemd
service using [systemd-generate](https://www.freedesktop.org/software/systemd/man/systemd.generator.html).
Once the service is started, it will pull all required images and run a podman
container.

It deprecates, but is not a direct alternative to, [podman-generate-systemd](https://docs.podman.io/en/latest/markdown/podman-generate-systemd.1.html).
It is more of an alternative to `docker-compose.yml` or `compose.yml` files.
So instead of writing compose files following the
[Compose Specification](https://compose-spec.io/), and instead of using
something like [docker compose](https://docs.docker.com/compose/) or [podman compose](https://github.com/containers/podman-compose), you write files in INI
format, similarly to a systemd unit.

It is not a direct alternative to podman-generate-systemd because it does not
generate the new format using your existing compose files. You need to manually
create new files based on them. Thankfully, there is a tool called
[Podlet](https://github.com/k9withabone/podlet) that is able to generate
container files for you based on `podman run`, `podman compose` or a
`{docker-}compose.yml`.

## Where are Quadlets stored?

Podman unit search path:

* /etc/containers/systemd/
* /usr/share/containers/systemd/

Podman user unit search path:

* ~/.config/containers/systemd/        <---- Only for your user
* /etc/containers/systemd/users/$(UID) <---- Only for a specific user
* /etc/containers/systemd/users/       <---- For all users

## What do you need to use Quadlets?

* [Podman 4.4.0](https://github.com/containers/podman/releases/tag/v4.4.0) or greater
* [Crun](https://github.com/containers/crun)
* [Have CGroupsV2 enabled](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/managing_monitoring_and_updating_the_kernel/using-cgroups-v2-to-control-distribution-of-cpu-time-for-applications_managing-monitoring-and-updating-the-kernel#mounting-cgroups-v2_using-cgroups-v2-to-control-distribution-of-cpu-time-for-applications)

## Learn more about Quadlets

* https://blogs.gnome.org/alexl/2021/10/12/quadlet-an-easier-way-to-run-system-containers/
* https://www.redhat.com/sysadmin/quadlet-podman
* https://www.redhat.com/sysadmin/multi-container-application-podman-quadlet
* https://www.linkedin.com/pulse/quadlets-how-i-learned-stop-worrying-love-dot-adam-clater
* https://docs.podman.io/en/latest/markdown/podman-systemd.unit.5.html

The real source-of-truth documentation is [podman-systemd.unit](https://docs.podman.io/en/latest/markdown/podman-systemd.unit.5.html).
You might also want to look at
[systemd.unit](https://www.freedesktop.org/software/systemd/man/systemd.unit.html)
and [systemd.service](https://www.freedesktop.org/software/systemd/man/systemd.service.html) to learn more about how to manage systemd services.

## Current caveats

The current documentation is scarce, and the only true documentation is a
manpage, namely [podman-systemd.unit](https://docs.podman.io/en/latest/markdown/podman-systemd.unit.5.html), which is far from ideal. There is the [docs folder of the original project](https://github.com/containers/quadlet/blob/main/docs/),
and while useful, it is severely outdated.

If your podman container setup relies on [Kubernetes Pods](https://kubernetes.io/docs/concepts/workloads/pods/) /
[Podman Pods](https://docs.podman.io/en/latest/markdown/podman-pod.1.html)
instead of [Docker Networks](https://docs.docker.com/network/), you are either
forced to write your own [Kubernetes Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) files, generate Kubernetes
Deployment files with [podman-kube-generate](https://docs.podman.io/en/latest/markdown/podman-kube-generate.1.html), or rewrite your setup to use Docker
Networks instead (the easiest option).

Depending on your Podman version, not all entries in `podman-systemd.unit`
might be supported. In that case, use the respective Podman command line flags
under the entry `PodmanArgs=`.

Unlike compose files which have built-in dependency management, this is not
present within the `[Container]` section of container files, and together with certain policies, such as `Restart=` or modifications to the container command
like `Exec=`, such features will have to be adapted to systemd unit style.

The following is a good read on how unit dependencies behave in systemd:
[Difference between PartOf and BindsTo in a systemd unit](https://pychao.com/2021/02/24/difference-between-partof-and-bindsto-in-a-systemd-unit/).

## Advantages of Quadlets

* Podman Pods require you to define ports and whatnot at Pod creation time,
which can be unpractical if you need to change your setup in the future. With
Quadlets, you alter the container file, reload the daemon and restart the service.

* Using podman-generate-systemd always generates complex and hard-to-read systemd service files. Quadlets do not have this issue.

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
[journald](https://www.freedesktop.org/software/systemd/man/systemd-journald.service.html) and are accessible via
[journalctl](https://www.freedesktop.org/software/systemd/man/journalctl.html), containers can be managed together with [systemd timers](https://www.freedesktop.org/software/systemd/man/systemd.timer.html), etc. This allows for capabilities that are not possible or are difficult to do with compose files.

* Because all processes related to the container are under the same service
cgroup, systemd is able to manage it entirely, and changing the respective
cgroup affects the entire container.

* Because the containers are internally run with podman, you can still manage
them with podman as you wish. This is useful, for instance, if the container
requires a restart after some internal modification to the container (like
MediaWiki), or if it needs changes to its matching database (like WriteFreely).
