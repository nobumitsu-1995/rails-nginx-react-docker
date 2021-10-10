# rails,postgresql,pume,nginx,react,dockerを使用した環境構築雛形
### docker-compose build

## Railsアプリの作成
1.　cd api. 
2.　docker-compose run api rails new . --api --force --no-deps --database=postgresql --skip-test. 
3.　config/puma.rbを以下のように編集. 
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
4.　config/database.ymlのdefault:を以下のように編集
`adapter: postgresql. 
    encoding: utf8. 
    pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>. 
    username: <%= ENV.fetch('POSTGRES_USER') { 'root' } %>. 
    password: <%= ENV.fetch('POSTGRES_PASSWORD') { 'password' } %>. 
    host: db`. 
5.　docker-compose run api rails db:create. 

## Reactアプリの作成
1.　cd front/frontend. 
2.　docker-compose run front npx create-react-app frontend --template=typescript. 

### docker-compose upで全てのイメージが立ち上がる。
