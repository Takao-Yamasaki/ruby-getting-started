FROM ruby:3.4

RUN apt-get update -qq && \
  apt-get install -y build-essential default-libmysqlclient-dev nodejs vim

WORKDIR /app

COPY Gemfile /app/Gemfile
COPY Gemfile.lock /app/Gemfile.lock

RUN bundle install

COPY . /app

# DDタグの設定
ENV DD_SERVICE=app
ENV DD_ENV=prod
ENV DD_VERSION=1.0

# Railsサーバーを起動
CMD ["bash", "-c", "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"]
