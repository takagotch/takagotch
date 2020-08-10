###### 
---

###### ActionText

```sh

```


```sh

```

######  ActionMailbox
```sh

```


```sh

```

###### rails db:system:change 
```sh


rails new apptky --database=postgresql
bin/rails g migration CreateUser name
bin/rails db:create db:migrate
bin/rails db:system:change --to-mysql
h // rails app:update
Y // Yes
bundle install
vi config/database.yml
bin/rails db:create db:migrate

show create table users\G
```

```sh
vi Gemfile
# gem 'mysql2'
vi database.yml
# adapter: mysql2
# encoding: utf8mb4
# pool: <% ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
# username: root
# password:
# socket: /var/lib/mysql.sock
rails db:system:change --to=mysql
bundle install


bin/rails db:system:change --to=postgresql
bundle
bin/rails db:create
bin/rails db:migrate
vi db/schema.rb
- t.integer "landmark_id", null: false
- t.integer "tag_id", null: false
+ t.bigint "landmark_id", null: false
+ t.bigint "tag_id", null: false
bin/rails db:import_tsv


```


```sh

```

