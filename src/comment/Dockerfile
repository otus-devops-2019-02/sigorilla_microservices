FROM ruby:2.2

WORKDIR /app
COPY . .

RUN bundle install

ENV COMMENT_DATABASE_HOST comment_db
ENV COMMENT_DATABASE comments

CMD ["puma"]
