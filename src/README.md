# Micorservices

## Run

```sh
docker network create reddit

docker run -d --network=reddit \
    --network-alias=post_host \
    --network-alias=comment_host \
    mongo:latest

docker run -d --network=reddit \
    -e POST_DATABASE_HOST=post_host \
    -e POST_DATABASE=posts \
    --network-alias=post_service \
    sigorilla/post:1.0

docker run -d --network=reddit \
    -e COMMENT_DATABASE_HOST=comment_host \
    -e COMMENT_DATABASE=comments \
    --network-alias=comment_service \
    sigorilla/comment:1.0

docker run -d --network=reddit \
    -e POST_SERVICE_HOST=post_service \
    -e POST_SERVICE_PORT=5000 \
    -e COMMENT_SERVICE_HOST=comment_service \
    -e COMMENT_SERVICE_PORT=9292 \
    -p 9292:9292 \
    sigorilla/ui:1.0
```
