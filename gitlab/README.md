## Running instructions

Start the container.

Once you are able to see the webpage on localhost:8080 and before 24 hours pass, run:

```bash
podman exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
```
