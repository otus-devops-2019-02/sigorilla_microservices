# Microservices

## Run

Для уменьшения размера образа использовал [multi-stage](https://docs.docker.com/develop/develop-images/multistage-build/).

```sh
docker network create reddit
docker volume create reddit_db

docker run -d --network=reddit \
    --network-alias=post_host \
    --network-alias=comment_host \
    -v reddit_db:/data/db \
    mongo:latest

docker run -d --network=reddit \
    -e POST_DATABASE_HOST=post_host \
    -e POST_DATABASE=posts \
    --network-alias=post_service \
    sigorilla/post:2.0

docker run -d --network=reddit \
    -e COMMENT_DATABASE_HOST=comment_host \
    -e COMMENT_DATABASE=comments \
    --network-alias=comment_service \
    sigorilla/comment:2.0

docker run -d --network=reddit \
    -e POST_SERVICE_HOST=post_service \
    -e POST_SERVICE_PORT=5000 \
    -e COMMENT_SERVICE_HOST=comment_service \
    -e COMMENT_SERVICE_PORT=9292 \
    -p 9292:9292 \
    sigorilla/ui:3.0
```
