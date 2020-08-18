###### docker rails
---

```.yml
- .dockerignore
- .env
- .gitignore
- Dockerfile
- Gemfile
- Makfile
- README.md
- config
  - database.yml
- docker-compose.yml

```

```sh
make build
make up
curl http://localhost:3000/
make down
make restart
make clean
make bi
make br
make dbc # rails db:create
make dbm # rails db:migrate
make dbs # rails db:seed
make rc  # rails c
make rr  # rails routes
make rt  # rails test


```

```database.yml
default: &default
  adapter: <%= ENV['DB_ADAPTER'] || 'postgresql' %>
  encoding: utf8
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: <%= ENV['DB_USER'] %>
  password: <%= ENV['DB_PASSWORD'] %>
  host: <%= ENV['DB_HOST'] %>
  
development:
  <<: *default
  database: <%= ENV['DB_DEV_NAME'] %>
  
test:
  <<: *default
  database: <%= ENV['DB_TEST_NAME'] %>
  
production:
  <<: *default
  database: <%= ENV['DB_PRODUCTION_NAME']%>
  username: <%= ENV['DB_PRODUCTION_USER'] %>
  password: <%= ENV['DB_PRODUCTION_PASSWORD'] %>
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




