FROM python:3.6.0-alpine

WORKDIR /app
COPY . .

RUN apk add --no-cache build-base gcc
RUN pip install -r ./requirements.txt

ENV POST_DATABASE_HOST post_db
ENV POST_DATABASE posts

CMD ["python3", "post_app.py"]
