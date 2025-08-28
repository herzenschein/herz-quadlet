# 📦herz-quadlet

The largest, best curated repository for Podman Quadlets on the internet.

All quadlets in this repo are rootless.

The main repository now resides in [Codeberg](https://codeberg.org/herzenschein/herz-quadlet),
the [Github](https://github.com/herzenschein/herz-quadlet) repository is now a mirror.
Issues can be reported in either.

> If you don't want to read any of this and just want to get started with Quadlets, see the [Quickstart](#quickstart).

## What are Quadlets?

Quadlets are a new way to manage containers. They consist of `.container` files.

By writing an INI file with the `.container` extension (as well as `.network`,
`.volume` and `.kube` if needed) and storing it in a certain directory,
[systemd](https://systemd.io/) is able to automatically generate a systemd
service using
[systemd-generate](https://www.freedesktop.org/software/systemd/man/systemd.generator.html).
Once the service is started, it will pull all required images and run a podman
container.

You can learn more about it in [Advantages of using Quadlets](Advantages.md).

You can learn more about how it works compared to different solutions in [Comparison between Quadlets and other solutions](Comparisons.md).

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

## 🚀Quickstart

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

## I want to...

If you're in doubt about how to achieve a particular task, see the [How To](HowTo.md).

## The expectations of this repository

You can read more about the reasoning behind the design choices in this repository and the guidelines on how to contribute to it in [Contributing](Contributing.md).
