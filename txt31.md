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



```docker-compose.yml

services:
  db:
    image: postgres
    ports:
     - "5432:5432"
    environment:
     - "POSTGRES_USER=postgres"
     - "POSTGRES_PASSWORD=password"
    volumes:
     - dbdata:/var/lib/postgresql/data
  app:
    build: .
    command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
    tty: true
    stdin_open: true
    ports:
     - "3000:3000"
    environment:
     - "DB_HOST=db"
     - "DB_PORT=5432"
     - "DB_USER=postgres"
     - "DB_PASSWORD=password"
    env_file: .env
    depends_on:
     - db
    volumes:
     - .:/opt/app:cached
     - bundle:/usr/local/bundle
     # exclude volumes
     - /opt/app/vendor
     - /opt/app/tmp
     - /opt/app/log
     - /opt/app.git

volumes:
  dbata:
  bundle:
```

```Dockerfile
FROM ruby: 2.7.1

ENV APP_ROOT=/opt/app/
ENV LANG C.UTF-8

WORKDIR $APP_ROOT

RUN apt-get update -qq && apt-get install -y build-essential --no-install-recommends \
                && rm -rf /var/lib/apt/lists/*
                
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
    && apt-get update \
    && apt-get install -y yarn \
    && rm -rf /var/lib/apt/lists/*

RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - \
    && apt-get install -y nodejs
    
COPY Gemfile Gemfile.lock $APP_ROOT

RUN echo 'gem: --no-document' >> ~/.gemrc \
    && cp ~/.gemrc /etc/gemrc \
    && chomd uog+r /etc/gemrc \
    && bundle config --global build.nokogiri --use-system-libraries \
    && bundle config --global jobs 4 \
    && bundle install \
    && yarn install \
    && rm -rf ~/.gem
    
COPY . $APP_ROOT    

EXPOSE 3000
```

```Gemfile
#
gem 'pg'
gem 'config'
gem 'dotenv-rails'
gem 'meta-tags'

group :development, :test do
  gem 'byebug', platforms: [:mri, :mingw, :x64-mingw]
  gem 'pry-byebug'
  gem 'pry-rails'
  gem 'pry-coolline'
  gem 'pry-stack_explorer'
  gem 'hirb'
  
  gem 'letter_opener'
  
  gem 'simplecov'
  
  gem 'breakman'
end

group :development do
  gem 'bullet'
  
  gem 'better_errors'
end
```



```Makefile
FIG = docker-compose
APP = $(FIG) exec app
RAILS = $(APP) rails

# container
build:
  @$(FIG) build

up:
  @$(FIG) up

down:
  @$(FIG) down

restart:
  @$(FIG) stop
  @$(FIG) start
  
clean:
  @docker system prune

# bundle install
bi:
  @$(APP) build install

br:
  @$(APP) gem uninstall -aIx
  @make bi
  
# rails 
rc:
  @$(RAILS) console
  
rr:
  @$(RAILS) routes

rt:
  @$(RAILS) test

# db
dbc:
  @$(RAILS) db:create
  
dbm: 
  @$(RAILS) db:migrate
  
dbs:
  @$(RAILS) db:seed

```

```.dockerignore
.bundle
.dockerignore
.env
.env.*
.git
log
node_modules
public/system
public/assets
public/packs
vendor/bundle
```

```.env
DB_USER=postgres
DB_PASSWORD=password
DB_HOST=db
DB_DEV_NAME=apptky_dev
DB_TEST_NAME=apptky_test
```

```.gitignore
.byebug_history
.rspec
.rvmrc

/.bundle

vendor/bundle/*

/public/assets/*
/public/packs
/public/packs-test
/node_modules

/db/*.sqlite3
/db/*.sqlite3-journal
/db//*.sqlite3-*

/log/*
/tmp/*
!/log/.keep
!/tmp/.keep
!.gitkeep
/spec/tmp/*

/yarn-error.log
yarn-debug.log*
.yarn-integrity

.env
config/settings.local.yml
config/settings/*.local.yml
config/environments/*.local.yml

/config/master.key

/storage/*
!/storage/.keep
/public/uploads

public/sitemap.xml.gz

.idea
*.iml

*.swp
*~
**.orig

converage
```

```
```




