FROM alpine:3.7 as build

RUN apk add --update --no-cache gcc make musl-dev ruby ruby-dev ruby-bundler ruby-rdoc ruby-irb \
    && gem install bundler

WORKDIR /tmp/app
COPY Gemfile* ./

RUN bundle install --binstubs=./bin/ --standalone

FROM alpine:3.7

ENV POST_SERVICE_HOST post
ENV POST_SERVICE_PORT 5000
ENV COMMENT_SERVICE_HOST comment
ENV COMMENT_SERVICE_PORT 9292

RUN apk add --no-cache --update ruby ruby-json

WORKDIR /app
COPY . .
COPY --from=build /tmp/app/bin ./bin/
COPY --from=build /tmp/app/bundle ./bundle/

ENV PATH /app/bin:$PATH

CMD ["ruby", "-r", "/app/bundle/bundler/setup.rb", "/app/bin/puma"]
