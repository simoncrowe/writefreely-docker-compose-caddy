# writefreely-docker-compose-caddy

This repo provides lightweight and easy way of deploying
(Writefreely)[https://github.com/writefreely/writefreely]
on a Linux server using docker.

It uses a convenient (writefreely docker image)[https://git.madhouse-project.org/algernon/writefreely-docker/src/branch/main]
that includes a configuration script and works _out of the box_ with a SQLite DB.
In the same vein it uses (Caddy)[https://caddyserver.com/] as a simple reverse proxy.
Caddy serves requests over HTTPS and automatically provisions SSL certificates.

## Getting Started

To use the repo, you need a linux server with git and docker on it.

Open a shell on the server and clone this repo:

```shell
git clone https://github.com/simoncrowe/writefreely-docker-compose-caddy
cd writefreely-docker-compose-caddy
```

Once in the root directory of the repo, you'll need to create two files:
`Caddyfile` and `.env`.


The first line of `Caddyfile` needs to be the domain name of your blog.
This allows Caddy to automatically set up HTTPs.
See [Caddy's documentation](https://caddyserver.com/docs/automatic-https)
for more on this.
The second line tells it to send HTTPs traffic to port 8080 on the
writefreely container.

```Caddyfile
example.com

reverse_proxy writefreely:8080
```

`.env` contains environment variables and their values to be
passed into the Writefreely container.
Some of the accepted environment variables are documented
[here](https://git.madhouse-project.org/algernon/writefreely-docker/src/branch/main#environment-variables).
For the rest, see [this shell script](https://git.madhouse-project.org/algernon/writefreely-docker/src/branch/main/bin/writefreely-docker.sh)
that serves as an entrypoint to the container.

The below keys and values should be enough to get a single-tenancy Writefreely
instance up and running

```env
WRITEFREELY_HOST="https://example.com"
WRITEFREELY_SINGLE_USER=true
WRITEFREELY_ADMIN_USER="admin"
WRITEFREELY_ADMIN_PASSWORD="A-STRONG-PASSWORD"
```
Note: despite the name `WRITEFREELY_HOST` must be a URL.
Providing just a hostname will cause the container to crash.

Most things can be customised once you've logged in as your admin user.

All that remains now is `docker compose up -d`.
Assuming your DNS records point to the server's IP
and firewalls allow ingress from ports 80 and 443, caddy should
talk to letsencrypt and soon you'll be able to access your Writefreely blog
over HTTPS.

If it doesn't work first-time, have a look at `docker compose logs -f`
and try to troubleshoot.

If you need to reconfigure the Writefreely container,
you can run `docker compose rm -f`
followed by `docker volume rm writefreely-docker-compose-caddy_wf_data`
to get rid of your old config and SQLite database. There's likely no harm
in re-using the Caddy SSL data volume.
