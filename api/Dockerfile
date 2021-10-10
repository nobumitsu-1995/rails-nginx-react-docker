FROM ruby:2.7.2

RUN set -x && \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends curl less sudo

ENV DOCKERIZE_VERSION v0.6.1
RUN set -x && \
    curl -sL https://github.com/jwilder/dockerize/releases/download/${DOCKERIZE_VERSION}/dockerize-linux-amd64-${DOCKERIZE_VERSION}.tar.gz | tar zx -C /usr/local/bin --no-same-owner --no-same-permissions

RUN mkdir /nginx-rails-react
WORKDIR /nginx-rails-react
COPY Gemfile /nginx-rails-react/Gemfile
COPY Gemfile.lock /nginx-rails-react/Gemfile.lock
RUN bundle install
COPY . /nginx-rails-react

COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3000

CMD ["rails", "server", "-b", "0.0.0.0"]

# puma.sockを配置するディレクトリを作成
RUN mkdir -p tmp/sockets