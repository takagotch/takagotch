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
```

######  ActionMailbox
```yml
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
    if mail.parts.present?
      Feedback.create(
        user_id: @user.id,
        product_id: product_id,
        content: mail.parts[0].body.decoded
      )
    else
      Feedback.create(
        user_id: @user.id,
        product_id: product_id,
        content: mail.decoded
      )
    end
  end
  
  def user
    @user ||= User.find_by(email: mail.from)
  end
  
  def product_id
    #- recipient = User.find_by(email: mail.from) { |r| RECIPIENT_FORMAT.match?(r) }
    #+ recipient = "feedback-123@gmail.com"
    recipient[RECIPIENT_FORMAT, l]
  end
end
%>
vi app/mailboxes/feedbacks_mailbox.rb
# routing FeedbackMailbox::RECIPIENT_FORMAT => :feedbacks
rails s
curl http://localhost:3000/users/new
curl http://localhost:3000/products/new
curl http://localhost:3000/rails/conductor/action_mailbox/inbound_emails/new
curl http://localhost:3000/rails/conductor/action_mailbox/inbound_emails/1
curl http://localhost:3000/products/1
vi app/products/show.html.erb
+ <p id="notice"><%= notice %></p>
+ <p>
+   <strong>Title:</strong>
+   <%= @product.title %>
+ </p>
+ <%= link_to 'Edit', edit_product_path(@product) %>
+ <%= link_to 'Back', products_path %>
+ <h4>Feedback</h4>
+ <%= render @product.feedbacks %>
vi app/feedbacks/_feedback.html.erb
+ <div>
+   <strong><%= feedback.user.name %></strong> said: <br />
+   <%= simple_format feedback.content %>
+ </div>
curl http://localhost:3000/products/1




vi app/products/show.html.erb
+ class FeedbacksMailbox < ApplicationMailer
+
+
+
+
+ def
+  
+ end
curl http://localhost:3000/rails/conductor/action_mailbox/inbound_emails/new
vi config/environment/production.rb
# config.action_mailbox.ingress = :postmark

# action_mailbox:
#   ingress_password: PASSWORD

# Demo Live
# https://www.freenom.com/en/index.html?lang=en
# https://sendgrid.com/
# https://ngrok.com/
curl https://actionmailbox:PASSWORD@gmail.com/rails/action_mailbox/postmark/inbound_emails
vi config/environment/development.rb
# config.action_mailer.smtp_settings = {
#   :user_name => SENDGRID_USERNAME,
#   :password => SENDGRID_PASSWORD,
#   :domain => OUR DOMAIN,
#   :address => 'smtp.sendgrid.net',
#   :port => 587,
#   :authentication => :plain,
#   :enable_starttls_auto => true
# }
# config.action_mailer.delivery_method = :smtp
# config.action_mailer.perform_deliveries = true
# config.action_mailer.raise_delivery_errors = false

./ngrok http 3000

# config.action_mailbox.ingress = :sendgrid

# action_mailbox:
#   ingress_password: PASSWORD

rails g mailer Feedback
vi app/mailers/feedback_mailer.rb
# class FeedbackMailer < ApplicationMailer
#   default from: FROM_MAIL_ADDRESS
#   def send_email
#     mail(to: ANY_USERES_EMAIL, reply_to: REPLY_TO_MAIL_ADDRESS,  subject: 'Mailbox Test', body: 'Provide feedback for the product by replying to this mail')
#   end
# end

FeedbackMailer.send_email.deliver_now
```


```sh

```


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

