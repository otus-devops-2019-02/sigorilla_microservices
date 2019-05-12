FROM alpine:3.7 as build

RUN apk add --update --no-cache gcc make musl-dev ruby ruby-dev ruby-bundler ruby-rdoc ruby-irb \
    && gem install bundler

WORKDIR /tmp/app
COPY Gemfile* ./

RUN bundle install --binstubs=./bin/ --standalone

FROM alpine:3.7

ENV COMMENT_DATABASE_HOST comment_db
ENV COMMENT_DATABASE comments

RUN apk add --no-cache --update ruby ruby-json

WORKDIR /app
COPY . .
COPY --from=build /tmp/app/bin ./bin/
COPY --from=build /tmp/app/bundle ./bundle/

ENV PATH /app/bin:$PATH

CMD ["ruby", "-r", "/app/bundle/bundler/setup.rb", "/app/bin/puma"]
