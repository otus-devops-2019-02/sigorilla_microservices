FROM alpine:3.7

ENV POST_DATABASE_HOST post_db
ENV POST_DATABASE posts

WORKDIR /app
COPY . .

RUN apk add --update --no-cache python3 && \
    apk add --update --no-cache --virtual tmp_build build-base gcc py3-pip python3-dev && \
    pip3 install -r ./requirements.txt && \
    apk del tmp_build

CMD ["python3", "post_app.py"]
