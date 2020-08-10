###### 
---

###### ActionText
```sh
rails new feedback_collector
rails action_mailbox:install
rails g scaffold User name email
rails g scaffold Product title
rails g scaffold Feedback user:references product:references context:text
rails db:migrate
vi app/mailboxes/application_mailbox.rb
# routing :all => :feedbacks
rails g mailbox Feedbacks


rails action_mailbox:install
rails g scaffold User name:string email:string
rails g scaffold Product title:string
rails g scaffold Feedback user:references
rails db:migrate
vi app/models/product.rb
vi app/models/user.rb
vi app/mailboxes/application_mailbox.rb
# routing /feedback\-(.+)@gmail.com/i => :feedbacks
rails g mailbox Feedbacks
vi app/mailboxes/feedbacks_mailbox.rb
# def process
# end
vi app/mailboxes/feedbacks_mailbox.rb
# def user
#   @user ||= User.find_by{email: mail.from}
# end
vi app/mailboxes/application_mailbox.rb
# RECIPIENT_FORMAT = /feedback\-(.+)@gmail.com/i
Vi app/mailboxes/feedbacks_mailbox.rb
<%#
class FeedbacksMailbox < ApplicationMailbox
  RECIPIENT_FORMAT = /feedback\-(.+)@gmail.com/i
  before_processing :user
  
  def process
  end
  
  def user
  end
  
  def product_id
    recipient = User.find_by(email: mail.from)
  end
end
%>
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


vi database.yml
# default: &default
#   adapter: postgresql
#   encoding: unicode
#   pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
#   
# development:
#   <<: *default
#   database: sample_development
#
# test:
#   <<: *default
#   database: sample_test
#
# production:
#   <<: *default
#   database: sample_production
#   username: sample
#   password: <%= ENV['SAMPLE_DATABASE_PASSWORD'] %>
vi Gemfile  
# gem 'pg'
rails db:system:change
rails db:system:change --to=postgresql
```


```sh

```

