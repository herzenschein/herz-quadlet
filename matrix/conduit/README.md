## Running instructions

Just change the `CONDUIT_SERVER_NAME`, then start the server.

Refer to the [Synapse Reverse Proxying](../synapse/README.md#reverse_proxying)
instructions to manage your Conduit Server and your Homeserver with a reverse
proxy. It has the same behavior.

Note that unlike Synapse, which is closed for registrations by default and
requires you to run `podman exec` to generate an admin user, Conduit defaults
to open registrations and the first user to register in it becomes admin.
