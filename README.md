# sigorilla_microservices

* https://hub.docker.com/u/sigorilla

## How to update `src`

```sh
cd src
git init
git remote add origin git@github.com:express42/reddit.git
git fetch
git checkout origin/<branch> -ft
rm -rf .git
cd ..
```

## Setup docker machine

```sh
GOOGLE_PROJECT=docker-239511 docker-machine create --driver google \
    --google-machine-image https://www.googleapis.com/compute/v1/projects/ubuntu-os-cloud/global/images/family/ubuntu-1604-lts \
    --google-machine-type n1-standard-1 \
    --google-zone europe-west1-b \
    docker-host
eval $(docker-machine env docker-host)
```

## Shut down docker machine

```sh
eval $(docker-machine env --unset)
docker-machine rm docker-host
```

## Host network driver

При запуске нескольких контейнеров с одной сетью `--network host` запущенным остается только первый, т.к. остальные не могут подключиться у нужному порту, ведь он уже занят.

> 2019/06/11 19:11:26 [emerg] 1#1: bind() to 0.0.0.0:80 failed (98: Address already in use)

## Docker compose

**Базовое имя проекта**

Оно генерируется либо с помощью опции `-p` CLI, либо с помощью `COMPOSE_PROJECT_NAME`.

## Gitlab

### Setup

```sh
sudo su
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
add-apt-repository "deb https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
apt-get update
apt-get install docker-ce docker-compose

mkdir -p /srv/gitlab/config /srv/gitlab/data /srv/gitlab/logs
cd /srv/gitlab/
touch docker-compose.yml # copy of ./gitlab-ci/docker-compose.yml

docker-compose up -d
```

### Setup runners

```sh
docker run -d --name gitlab-runner --restart always \
    -v /srv/gitlab-runner/config:/etc/gitlab-runner \
    -v /var/run/docker.sock:/var/run/docker.sock \
    gitlab/gitlab-runner:latest
docker exec -it gitlab-runner gitlab-runner register --run-untagged --locked=false
# http://<YOUR-VM-IP>/
# <TOKEN>
# linux,xenial,ubuntu,docker
# docker
# alpine:latest
```
