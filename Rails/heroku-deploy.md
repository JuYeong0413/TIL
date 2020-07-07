# Rails 프로젝트 Heroku에 배포하기  

## 배포 전 사전 작업  
### 1. [Heroku](https://www.heroku.com/) 회원가입 및 로그인  
### 2. [Heroku CLI 설치](https://devcenter.heroku.com/articles/getting-started-with-python#set-up)  
### 3. Database 변경  
`Heroku`에서는 `sqlite3`사용이 불가능하기 때문에 `postgresql`로 database를 변경해야 한다.  
#### Gemfile  
```
gem 'sqlite3'
```
부분을  
```
gem 'pg'
```
로 바꿔준다.  
```
$ bundle install
```
해주고  

#### database.yml  
```yaml
# SQLite version 3.x
#   gem install sqlite3
#
#   Ensure the SQLite 3 gem is defined in your Gemfile
#   gem 'sqlite3'
#
default: &default
  # adapter: sqlite3
  adapter: postgresql
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  timeout: 5000

development:
  <<: *default
  # database: db/development.sqlite3
  database: myproject_development

# Warning: The database defined as "test" will be erased and
# re-generated from your development database when you run "rake".
# Do not set this db to the same as development or production.
test:
  <<: *default
  # database: db/test.sqlite3
  database: myproject_test

production:
  <<: *default
  # database: db/production.sqlite3
  database: myproject_production

```
`sqlite3`로 되어있던 `database.yml` 파일도 `postgresql`로 수정해준다.  
```bash
$ rails db:create
```
```bash
$ rails db:migrate
```
서버 실행해보고 정상적으로 작동된다면 데이터베이스 변경 완료  
### 4. `Github` repository의 `master` branch에 모든 변경사항 `push`하기  

## Heroku에 배포하기  
### 1. Heroku 로그인  
```bash
$ heroku login
```
### 2. Heroku app 생성  
```bash
$ heroku create
```
### 3. Heroku에 push  
```bash
$ git push heroku master
```
### 4. database migrate  
```bash
$ heroku run rake db:migrate
```
### 5. 배포된 홈페이지 확인  
```bash
$ heroku open
```
