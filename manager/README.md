# yadd/lemonldap-ng-portal

Lemonldap::NG portal based on [yadd/lemonldap-ng-base](https://github.com/guimard/llng-docker/blob/master/base/README.md#readme)

Note that you should share sessions and configuration to use. See
docker-compose example to see how to do this using redis and
[PostgreSQL](https://github.com/guimard/llng-docker/blob/master/pg/README.md#readme).

## Tags

* `stable`: latest lemonldap-ng\* packages from Debian backports
* `2.x.x`: versioned lemonldap-ng\* packages from Debian backports

## Features _(inherited from [yadd/lemonldap-ng-base](https://github.com/guimard/llng-docker/blob/master/base/README.md#readme))_

* Update current configuration using given variables :
  * set domain (`SSODOMAIN`)
  * set portal (`PORTAL`)
  * set log level (`LOGLEVEL`)
  * if `REDIS_SERVER` is set, change `globalStorage` to `Apache::Session::Browseable::Redis` and configure it _(indexes given by `REDIS_INDEXES`, default: "uid mail")_
* Upload local configuration into PostgreSQL database if:
  * `PG_SERVER` is given AND
  * PostgreSQL table is empty

## Variables and default values

See [yadd/lemonldap-ng-base](https://github.com/guimard/llng-docker/blob/master/base/README.md#readme)

## Docker-compose example

```yaml
version: "3.4"

services:
  db:
    image: yadd/lemonldap-ng-pg-database
    environment:
      - POSTGRES_PASSWORD=zz
    healthcheck:
      test: "exit 0"
  redis:
    image: redis
  portal:
    image: yadd/lemonldap-ng-portal
    environment:
      - PG_SERVER=db
      - REDIS_SERVER=redis:6379
      - LOGGER=stderr
      - USERLOGGER=stderr
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_started
  manager:
    image: yadd/lemonldap-ng-manager
    environment:
      - PG_SERVER=db
      - REDIS_SERVER=redis:6379
      - LOGGER=stderr
      - USERLOGGER=stderr
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_started

  haproxy:
    image: haproxy:2.6-bullseye
    ports:
      - 80:80
    volumes:
      - ./haproxy:/usr/local/etc/haproxy:ro
    sysctls:
      - net.ipv4.ip_unprivileged_port_start=0
    depends_on:
      - portal
      - manager
```

## Repository and bug reports

* Repository: [github.com/guimard/llng-docker](https://github.com/guimard/llng-docker/tree/master/manager)
* [Dockerfile](https://github.com/guimard/llng-docker/blob/master/manager/Dockerfile)
* [Issues database](https://github.com/guimard/llng-docker/issues)

## Copyright and license

Copyright: Xavier (Yadd) Guimard <yadd@debian.org>.

License: [GNU General Public License v2.0](https://github.com/guimard/llng-docker/blob/master/LICENSE)
