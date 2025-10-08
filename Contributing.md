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
* All quadlets are supposed to log to systemd unless you have set it up to not do that (LogDriver)

## Licensing

Any contributions are automatically assigned a GPL-3.0-or-later license unless you specifically [add an SPDX entry](https://community.kde.org/Guidelines_and_HOWTOs/Licensing) to your contributed file. The license should be FOSS, whether that's copyleft or permissive. The licensing choice implies some things:

* Nothing will affect the average user (including companies), as Quadlets are primarily for internal use (and the GPL does not require anything from you for internal use)
* In the extremely unlikely scenario where companies distribute Quadlets, they should make any modifications public instead of being closed-source leeches
* Other people wanting to modify Quadlets I wrote for their own distribution are *technically* supposed to also make them GPLv3, but in practice what I want is that, like me, you also keep them open source and accessible

## Contribution guidelines

Quadlets must always Just Work. This means they need to be tested before being added to this repository. I have personally tested every single Quadlet in this repository.

Quadlets must be simple by default.
Any highly specific or complex customizations should go to the [How To](HowTo.md).
This includes [Kube quadlet units](docs.podman.io/en/latest/markdown/podman-systemd.unit.5.html#pod-units-pod)
or Quadlet [Pod quadlet units](docs.podman.io/en/latest/markdown/podman-systemd.unit.5.html#pod-units-pod).

Container images must be at least *mostly qualified*.
This means they need to have the domain of the website that hosts the image,
whether that is Dockerhub's <docker.io>, Quay's <quay.io>, Github's <ghcr.io>, or something else.
This is a [best practice](https://www.redhat.com/en/blog/be-careful-when-pulling-images-short-name) in the container world.
In addition to that, you want to ensure that the user is getting the right first-party image where possible
instead of some random third-party container image.

Container names must follow the upstream name *in some way*. It doesn't need to be exactly the same.
For example, `docker-mailserver` is the upstream name, but I use `dockermailserver` instead.

The container name for the main user-facing service
should be just the upstream name instead of generic names like `web` or `app`.
Other necessary components get the upstream name + `-suffix`, for example, `mediawiki-db`.
It should always match the `ContainerName=` entry.
This is because this repository doesn't default to Pods,
and because it renders nice container names in terminal output,
making it more user friendly.

If a container requires a database, use either the simplest solution (often SQLite) or the upstream-preferred solution (often PostgreSQL or MariaDB), but choose only one. Other container databases should simply have their own Quadlet directory and have proper documentation such that they can be easily used with any Quadlet that requires them.

Only reverse proxies should make direct use of the 80 and 443 outbound ports.
For example, Cinny publishes the inbound port 80, but it is mapped to the outbound port 8080.
This makes it less annoying to test the containers locally,
as using these ports requires `net.ipv4.ip_unprivileged_port_start=80`.
