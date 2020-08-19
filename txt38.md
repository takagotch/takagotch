###### rails6 deploy heroku
---



```config/puma.rb
# http://localhost:3000/
if ENV.fetch('RAILS_ENV') { 'development' } == 'development'
  ssl_bind '0.0.0.0', '3000', key: './localhost-key.pem', cert: './localhost.pem'
end


```

```sh
brew install heroku/brew/heroku
brew tap heroku/brew && brew install heroku


heroku apps:create
heroku apps:destroy --app apptky
git remote -v
heroku addons:create cleardb:ignite
heroku config

heroku config:add DB_NAME='dbtky'
heroku config:add DB_USERNAME="tky"
heroku config:add_DB_PASSWORD="password"
heroku config:add_DB_HOSTNAME="dbhost"
heroku config:add DB_PORT="3306"

git push heroku master
heroku run rails db:migrate
heroku apps:info


```

```sh
postgres -D /usr/local/var/postgres
rails new memberlist -d postgresql
cd apptky
rails s

createdb apptky_development
rails g scaffold Member name:string comment:text
rake db:migrate
rails s

git init
git add .
git commit -m '1st'

vi Gemfile
# gem 'rails_12factor', group: :production
bundle install
vi Procfile
git commit -m 'procfile'
heroku create
git remote -v
git push heroku master

heroku addons:add heroku-postgresql
heroku run rake db:migrate
heroku apps:info
```


```
```

```
```

```
```


```
```

```
```

```
```


```
```

```
```

```
```


```
```

```
```

```
```


```
```

```
```

```
```



