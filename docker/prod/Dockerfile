FROM ruby:3.4

RUN apt-get update -qq && \
  apt-get install -y build-essential default-libmysqlclient-dev nodejs vim

WORKDIR /app

COPY Gemfile /app/Gemfile
COPY Gemfile.lock /app/Gemfile.lock

RUN bundle install

COPY . /app

# TODO: Datadog導入時にコメントアウト解除
# DDタグの設定
# ENV DD_SERVICE=app
# ENV DD_ENV=prod
# ENV DD_VERSION=1.0

# 本番環境設定
ENV RAILS_ENV=production
ENV RACK_ENV=production

# Railsサーバーを起動
# HerokuのPORT環境変数を使用し、本番環境で起動
CMD ["bash", "-c", "rm -f tmp/pids/server.pid && bundle exec puma -C config/puma.rb"]
