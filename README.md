# rails,postgresql,pume,nginx,react,dockerを使用した環境構築雛形
### `docker-compose build`でイメージを構築

## Railsアプリの作成
1.cd api<br>
2.docker-compose run api rails new . --api --force --no-deps --database=postgresql --skip-test<br>
3.config/puma.rbを以下のように編集<br>
```ruby:config/puma.rb
threads_count = ENV.fetch("RAILS_MAX_THREADS") { 5 }.to_i
threads threads_count, threads_count
port        ENV.fetch("PORT") { 3000 }
environment ENV.fetch("RAILS_ENV") { "development" } 
plugin :tmp_restart
app_root = File.expand_path("../..", __FILE__) 
bind "unix://#{app_root}/tmp/sockets/puma.sock"
stdout_redirect "#{app_root}/log/puma.stdout.log", "#{app_root}/log/puma.stderr.log", true
```
4.config/database.ymlのdefault:を以下のように編集<br>
```ruby:config/database.yml
adapter: postgresql
encoding: utf8
pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
username: <%= ENV.fetch('POSTGRES_USER') { 'root' } %>
password: <%= ENV.fetch('POSTGRES_PASSWORD') { 'password' } %>
host: db
``` 
5.　docker-compose run api rails db:create

## Reactアプリの作成
1.`cd front/frontend`<br>
2.`docker-compose run front npx create-react-app frontend --template=typescript`<br>

### `docker-compose up`で全てのイメージが立ち上がる。
