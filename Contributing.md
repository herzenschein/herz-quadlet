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

## Contribution guidelines
