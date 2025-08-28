## How To

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
