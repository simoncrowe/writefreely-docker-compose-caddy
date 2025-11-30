# writefreely-docker-compose-caddy

This repo provides lightweight and easy way of deploying Writefreely
on a Linux server using docker.

It uses a convenient (writefreely docker image)[https://git.madhouse-project.org/algernon/writefreely-docker/src/branch/main]
that includes a configuration script and works _out of the box_ with a SQLite DB.
In the same vein it uses (Caddy)[https://caddyserver.com/] as a simple reverse proxy.
Caddy serves requests over HTTPS and automatically provisions SSL certificates.
