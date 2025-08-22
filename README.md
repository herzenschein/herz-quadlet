# herz-quadlet
A repository where I store my podman quadlets.

All quadlets in this repo are rootless.

The main repository now resides in [Codeberg](https://codeberg.org/herzenschein/herz-quadlet),
the [Github](https://github.com/herzenschein/herz-quadlet) repository is now a mirror.
Issues can be reported in either.

## The expectations of this repository

This repository is rather opinionated:

* Only the `latest` tag is used, unless it's not possible to do so
  * It's a moving target, so container changes are caught faster
  * An up-to-date service is a service with all patches and CVEs covered ASAP
* The quadlets are designed for rootless podman, not rootful podman
* The directory structure is like this so you can simply copy the folder of the project you want to ~/.config/containers/systemd
  * Podman supports an extra directory level in ~/.config/containers/systemd
* Kube quadlets are not used

All quadlets available here are expected to follow this behavior:

* If the container file contains "generate", "build", or "init" in its name, it's not supposed to restart
* All other container files are supposed to start on boot (default.target)
* If a service has dependencies, those dependencies should automatically start before the main service (WantedBy)
* If a service has dependencies, when the service crashes or stops, all its dependencies should stop (BindsTo)
* All quadlets are supposed to force log to systemd even if you have set it up to not do that (LogDriver)

## What are Quadlets?

Quadlets are a new way to manage containers. They consist of `.container` files.

By writing an INI file with the `.container` extension (as well as `.network`,
`.volume` and `.kube` if needed) and storing it in a certain directory,
[systemd](https://systemd.io/) is able to automatically generate a systemd
service using
[systemd-generate](https://www.freedesktop.org/software/systemd/man/systemd.generator.html).
Once the service is started, it will pull all required images and run a podman
container.

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

## Where are Quadlets stored?

Podman unit search path:

* /etc/containers/systemd/
* /usr/share/containers/systemd/

Podman user unit search path:

* ~/.config/containers/systemd/        <---- Only for your user
* /etc/containers/systemd/users/$(UID) <---- Only for a specific user
* /etc/containers/systemd/users/       <---- For all users

If you have no idea how to get started or you are using rootless Podman, store
your container files in ~/.config/containers/systemd/ and run them as a user.

## Quickstart

This quickstart assumes you will be using rootless containers. If you want to
use rootful podman quadlets, change the images and configurations accordingly,
as those might vary according to each upstream project.

1. Create the folder `~/.config/containers/systemd/`
2. Copy the `owncast/` folder to `~/.config/containers/systemd/`
3. Run `systemctl daemon-reload --user`
4. Run `systemctl start --user owncast`

Done, you now have your own selfhosted [Owncast](https://owncast.online/)
instance! You can visit it under `localhost:8080` and configure it under
`localhost:8080/admin`.

Additionally, the following commands might be useful to you:

* `systemctl status --user owncast`: check the Owncast service status
* `systemctl list-unit-files --user --all --state generated`: list all
generated services
* `journalctl --user-unit owncast`: see the logs for your Owncast container
* `podman stats owncast`: monitor the stats of your Owncast container
* `journalctl enable-linger youruser`: let systemd services run while your
user is not logged in

## Automatic updates

Unlike Docker which requires additional software like Watchtower, Podman Quadlets come with the functionality to fetch and update containers by default.

The container name must be fully qualified: that means something like `docker.io/owncast/owncast:latest`.

You can either manually run `podman auto-update` or enable its respective systemd service: `sudo systemctl enable podman-auto-update`.

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

The real source-of-truth documentation is
[podman-systemd.unit](https://docs.podman.io/en/latest/markdown/podman-systemd.unit.5.html).
You might also want to look at
[systemd.unit](https://www.freedesktop.org/software/systemd/man/systemd.unit.html) and
[systemd.service](https://www.freedesktop.org/software/systemd/man/systemd.service.html)
to learn more about how to manage systemd services.

## Current caveats

* Depending on your Podman version, not all entries in `podman-systemd.unit`
might be supported. In that case, use the respective Podman command line flags
under the entry `PodmanArgs=`.

* Unlike compose files which have built-in dependency management, this is not
present within the `[Container]` section of container files, and together with
certain policies, such as `Restart=` or modifications to the container command
like `Exec=`, such features will have to be adapted to systemd unit style.
You might want to use
[WantedBy=default.target](https://docs.podman.io/en/latest/markdown/podman-systemd.unit.5.html#enabling-unit-files)
in the `[Install]` section to start a service on boot.
The following is a good read on how unit dependencies behave in systemd:
[Difference between PartOf and BindsTo in a systemd unit](https://pychao.com/2021/02/24/difference-between-partof-and-bindsto-in-a-systemd-unit/).

## Advantages of Quadlets

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
