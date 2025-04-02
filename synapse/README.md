## Running instructions

This is a minimal example using PostgreSQL.
This is in preparation for when Synapse comes with SlidingSync by default.

With this setup, you'll have your Synapse server in `matrix.example.com`
but your Homeserver will be `example.com`, so your users will show up as
`@user:example.com`.

```
systemctl daemon-reload --user
systemctl start --user synapse-generate
# Edit the generated data/homeserver.yaml file
systemctl start --user synapse-postgres
# If synapse-postgres fails to start, start it again and it will work
systemctl start --user synapse
podman exec --interactive --tty synapse register_new_matrix_user http://localhost:8008 -c /data/homeserver.yaml
```

Required changes to the YAML file:

```yaml
database:
  name: psycopg2
  args:
    user: synapse-postgres
    password: <passwordSetInSynapsePostgresContainerFile>
    dbname: synapse-postgres
    host: synapse-postgres
    cp_min: 5
    cp_max: 10
```

You can generate a random password with `pwgen --secure 13 1`.

Recommendations to put in the YAML file:

* `enable_registration: false` (after you add your main admin user)
* `enable_authenticated_media: true` (to avoid unauthenticated media issues)
* `use_presence: false` (decreases server load by not showing when you're online)

## Server naming and subdomains

Let's distinguish between two words to understand this:

* Homeserver: the name that appears after the username, as in `@user:homeserver`.

* Synapse Server: the name of where the actual Synapse server is hosted, which can be
a domain or subdomain. This can be in a subdomain, like `matrix.example.com`.

The current Synapse docs do not make an implicit distinction between what is
traditionally called a Server and what is a Synapse Server. When the Synapse
documentation says "Server", it always means "Synapse Server"

There are two ways you will want to set Matrix up:

* (1) the Homeserver name matches the Synapse server: `matrix.example.com` is
where Synapse is hosted, and users will be named `@user:matrix.example.com`.

* (2) the Homeserver does not match the Synapse server: `@user:example.com` is
how users will be named, yet `matrix.example.com` is where Synapse is installed
and running.

The [Synapse docs for Delegation](https://matrix-org.github.io/synapse/develop/delegate.html)
call (2) an "External Server".

The environment variable SYNAPSE_SERVER_NAME refers to the domain or subdomain
where Synapse is hosted. It matches the generated `/data/homeserver.yaml`
key `server_name`. It also matches the API call for
[GET /.well-known/matrix/client](https://spec.matrix.org/v1.8/client-server-api/#getwell-knownmatrixclient),
namely `m.server`, and
[GET /.well-known/matrix/server](https://spec.matrix.org/v1.8/server-server-api/#getwell-knownmatrixserver),
namely `m.homeserver`.

In other words: `SYNAPSE_SERVER_NAME`, `server_name`, `m.server` and
`m.homeserver` should refer to the same place, namely the Synapse Server.
If one of these does not match, you will get the `MatchingServerName` error
once you try the [Matrix Federation Tester](https://federationtester.matrix.org/).

In order for the Matrix server to delegate, it needs two files to exist:
`/.well-known/matrix/server` and `/.well-known/matrix/client`. This can occur
under (1) or under (2). Typically you will use a web server/reverse proxy for this.

## Reverse proxying

Under the (1) scenario (the one where Homeserver and Synapse Server match,
so you get that ugly `@user:matrix.example.com` username),
the instructions that you'd use for e.g. Caddy would look like this:

```
matrix.example.com {
    reverse_proxy /_matrix/* synapseIP:port
    reverse_proxy /_synapse/client/* synapseIP:port

    header /.well-known/matrix/* Content-Type application/json
    header /.well-known/matrix/* Access-Control-Allow-Origin *
    respond /.well-known/matrix/server `{"m.server": "matrix.example.com:443"}`
    respond /.well-known/matrix/client `{"m.homeserver": {"base_url": "https://matrix.example.com"}}`
}
```

The respond command replies with the necessary JSON for delegation, so you don't
need to create the file yourself.

I found this under the [Conduit repo](https://gitlab.com/famedly/conduit/-/issues/313).
Kudos to the issue reporter!

Under the (2) scenario (the one where Homeserver and Synapse Server do not match,
which is usually more desirable to most people because of nicer usernames
`@user:example.com`), it is a bit more complicated.

Let's say we want the Homeserver to be `example.com`, and the Synapse Server is
hosted under `matrix.example.com`.

Other servers will typically find you by default under port 8448 without you
really needing to do much. In other words, if you expose port 8448, it should
just work. This is the first example in the
[reverse proxy documentation](https://matrix-org.github.io/synapse/develop/reverse_proxy.html):

```
example.com:8448 {
  reverse_proxy /_matrix/* synapseIP:8008
}

matrix.example.com {
  reverse_proxy /_matrix/* synapseIP:8008
  reverse_proxy /_synapse/client/* synapseIP:8008
}
```

Note how a reverse proxy managed by Caddy is necessary for `example.com:8448`
to point to `matrix.example.com`.

However, the `.well-known/matrix/server` and `.well-known/matrix/client` files
can simply be available under port 443, and they will be checked by default
via an API call. It is used for the *Homeserver*. This can be convenient for
(2) so no additional port is exposed.

If a different server pings your *Homeserver*, `example.com`, and finds
`example.com/.well-known/matrix/server` and
`example.com/.well-known/matrix/client`, it will fetch the response you set:
namely `m.server` and `m.homeserver`, which should point to `matrix.example.com`.
This is the second example:

```
example.com {
    header /.well-known/matrix/* Content-Type application/json
    header /.well-known/matrix/* Access-Control-Allow-Origin *
    respond /.well-known/matrix/server `{"m.server": "matrix.example.com:443"}`
    respond /.well-known/matrix/client `{"m.homeserver":{"base_url":"https://matrix.example.com"},"m.identity_server":{"base_url":"https://identity.example.com"}}`
}

matrix.example.com {
    reverse_proxy /_matrix/* synapseIP:8008
    reverse_proxy /_synapse/client/* synapseIP:8008
}
```

So it's the response call that is doing the redirect to your Synapse Server,
rather than your reverse proxy. This is why the example offers no reverse proxy
for `example.com`.

## External resolution issues

If you use a service like Cloudflare which provides their own proxying for your
website, you might find that using `localhost` will not work.

Similarly, if you use a too old version of Podman, without up-to-date `netavark`
and `aardvark-dns` which are the next generation network stack which fixes many
DNS resolution matters for Podman containers, simply calling the container
name (say, `synapse:8008`) might not work for this either.

The surefire way to resolve these two issues can be to point directly to the
server IP instead.
