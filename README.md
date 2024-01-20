# Bacula 13.0.3 Container

Deploy the bacula community edition on Docker Containers. 

## Images

- [x] Bacula Catalog                    gpmidi/bacula-catalog:13.0.3
- [x] Bacula Director                   gpmidi/bacula-director:13.0.3
- [x] Bacula Storage Daemon             gpmidi/bacula-storage:13.0.3
- [x] Bacula File Daemon                gpmidi/bacula-client:13.0.3
- [x] Baculum Web Gui                   gpmidi/baculum-web:13.0.3
- [x] Baculum API                       gpmidi/baculum-api:13.0.3
- [ ] Postfix SMTP Relay                FIXME
- [ ] SMTP2TG SMTP Relay to Telegram    FIXME

## Install Docker 

    curl -sSL https://get.docker.com | bash
    
## Install Docker-compose

    curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose

## Download and Install Bacula Container

    git clone https://github.com/gpmidi/bacula
    cd bacula/docker
    docker-compose up

## Tests

    docker exec -it docker_bacula-dir_1 bash
    > bconsole
    * 
    
## Build

### Base Docker image

```shell
cd bacula-base
BACULA_KEY=<put-bacla-repo-key-here> docker buildx build --load \
       --no-cache \
       --tag gpmidi/bacula-base:latest \
       --platform linux/amd64 \
       --build-arg EL_VERSION=8 \
       --build-arg BACULA_KEY=${BACULA_KEY} \
       --build-arg BACULA_VERSION=13.0.3 \
       -f Dockerfile .
```
    
## Video

[![asciicast](https://asciinema.org/a/279317.svg)](https://asciinema.org/a/279317)


## Docker Compose

docker-compose.yaml


    version: '3.1'
    services:
      db:
        image: gpmidi/bacula-catalog:13.0.3
        restart: unless-stopped
        environment:
          POSTGRES_PASSWORD: bacula
          POSTGRES_USER: bacula
          POSTGRES_DB: bacula
        volumes:
        - pgdata:/var/lib/postgresql/data:rw
        ports:
          - 5432
      bacula-dir:
        image: gpmidi/bacula-director:13.0.3
        restart: unless-stopped
        volumes:
          - ./etc/bacula-dir.conf:/opt/bacula/etc/bacula-dir.conf:ro
          - ./etc/bconsole.conf:/opt/bacula/etc/bconsole.conf:ro
        depends_on:
          - db
        ports:
          - 9101
      bacula-sd:
        image: gpmidi/bacula-storage:13.0.3
        restart: unless-stopped
        depends_on:
          - bacula-dir
          - db
        volumes:
          - ./etc/bacula-sd.conf:/opt/bacula/etc/bacula-sd.conf:ro
        ports:
          - 9103
      bacula-fd:
        image: gpmidi/bacula-client:13.0.3
        restart: unless-stopped
        depends_on:
          - bacula-sd
          - bacula-dir
        volumes:
          - ./etc/bacula-fd.conf:/opt/bacula/etc/bacula-fd.conf:ro
        ports:
          - 9102
    volumes:
      pgdata:


## Reference

* https://github.com/fametec/bacula
* http://www.bacula.lat/community/baculum/ 
* http://www.bacula.lat/community/script-instalacao-bacula-community-9-x-pacotes-oficiais/
* https://www.bacula.org/documentation/documentation/
