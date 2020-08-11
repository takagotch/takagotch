###### ActiveRecord::Base.transaction

```
ActiveRecord::Base.transaction do
  question.save!
  answer.save!
end

```
###### Active Storage
```nginx.conf
server {
  listen 443 ssl;
  server_name tkgcci.com;
  
  location /rails-demo {
    try_fies $uri @apptky;
  }
  location @rails_demo_app {
    proxy_set_header X-Real_IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy-add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_pass http://localhost:3000;
  }
}

```

```config/application.rb
config.active_storage.routes_prefix = '/apptky'

```

```url
https://tkgcci.com/apptky/blobls/*

https://tkgcci.com/apptky/
```

###### [2-4]window, workspace
```
Home + RightLeft

Home + Alt + RightLeft
```

```
```

###### multi models
| | | |
| :---: | :---: | :---: |
| id | integer| |
| title | string| |

||||
| :---: | :---: | :---: |
| id | integer||
| question_id | integer| table ID|
| name | string | name|
| url | string | URL |
| body| string| BODY CONTENT |
||||
||||

```rb
def new
  @question = Question.new
  @answer = Answer.new
end

def create
  @question = Question.new(question_params)
  @answer = Answer.new(answer_params[:answer])
end

private

def question_params
  params.require(:question).permit(:title)
end

def answer_params
  params.require(:question).permit(answer:[:name, :url, :body])
end

```

```.slim
= form_with model: @question,local:true do |f|
  .form-group
   = f.label :tile
   = f.text_field :title, class 'form-control', id: 'question_title'
   
 = f.fields_for @answer do |i|
   .form-group
     = i.label :name
     = i.text_field :name, class: 'form-control', id: 'question_answer_name'
   .form-group
     = i.label :url
     = i.text_field :url, class: 'form-control', id: 'question_answer_url'
   .form-group
     = i.label :body
     = i.text_area :body, rows: 5, class: 'form-control', id: 'question_answer_body'

= f.submit 'CREATE', class: 'btn btn-primary'
```

```json
{
// output
}
```
```html
// output
```

###### puma_worker_killer, jemalloc

```
vi Gemfile
# gem 'puma_worker_killer'
```


```config/puma.rb
befor_fork do
  PumaWorkerKiller.config do |config|
    config.ram = 1837
    
    config.frequency = 24 * 60 * 60
    
    config.percent_usage = 0.90
    
    config.rolling_restart_frequency = 3 * 60
    
    config.reaper_status_logs = false
  end
  PumaWorkerKiller.start
end
```


###### CRUD rails,react,ajax

```sh
cd ~/ && mkdir apptky && cd apptky
rails new . --webpack=react --skip-turbolinks --skip-action-mailer --skip-action-mailbox --skip-active-storage --skip-test -d mysql
yarn add react-app-polyfill
yarn add date-fns
yarn add formdata-polyfill

bin/rails g model react_crud_data name:string comment:text
bin/rails g model react_crud_data_bk name:string comment:text

bin/rails db:migrate
bin/rails db:seed

bin/rails g controller react_crud_data index

```

```Gemfile
gem 'bootstrap'
# app/assets/stylesheets/application.css
# app/assets/stylesheets/application.scss
# application.scss
@import "bootstrap"
```

```config/application.rb
class Application < Rails::Application
  config.time_zone = 'Asia/Osaka'
end
```

```config/database.yml
```

```db/seeds.rb

```

```config/routes.rb
Rails.application.routes.draw do
  root to: 'react_crud_data#index'
  
  get 'react_crud_data/index'
  get 'react_crud_data/new', to: 'react_crud_data#new', as:'new_react_crud_data'
  post 'react_crud_data', to: 'react_crud_data#create'
  put 'react_crud_data/:id', to: 'react_crud_data#update'
  delete 'react_crud_data/:id', to: 'react_crud_data#destroy'
end
```

```app/views/layouts/application.html.erb
<!DOCTYPE html>
<html lang="ja">
  <head>
  </head>
</html>
```

```app/views/react_crud_data/index.html.erb
<div id="root"></div>
```

```app/controllers/react_crud_data_controller.rb

```

```app/models/react_crud_datum.rb
```

```a
```

```
```




```app/javascript/packs/hello_react.jsx
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


######
```
```

```
```


######
```
```

```
```


######
```
```

```
```


######
```
```

```
```


###### devise
```sh
cd ~/ && mkdir apptky && cd apptky
rails new . --skip-turbolinks --skip-test -d mysql


mysql -u root -p
CREATE USER 'tky' IDENTIFIED BY 'password';
CREATE DATABASE apptky_development;
GRANT ALL PRIVILEGES ON apptky_development.* TO 'tky';

bin/rails g model Cat name:string description:text
bin/rails db:migrate
bin/rails g controller cats index show new edit

vi Gemfile
# gem 'devise'
# gem 'devise-i18n'
# gem 'devise-i18n-views'
bundle install

rails g devise:install

rails g devise:i18n:views
rails g devise:views:locale ja

rails g devise:controllers users

bin/rails db:migrate

curl http://localhost:3000/
```

```config/database.yml
development:
  <<: *default
  database: apptky_development
  username: apptky_development
  password: password
```

```config/environments/development.rb
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
```

```config/routes.rb
Rails.application.routes.draw do
  get 'cats/index'
  get 'cats/show'
  get 'cats/new'
  get 'cats/edit'
  
  root to: 'cats#index'
end
```

```app.views/layouts/application.html.erb
<!DOCTYPE html>
<html>
  <head>
  <%= csrf_meta_tags %>
  <%= csp_meta_tag %>
  
  <%= stylesheet_link_tag 'application', media: 'all' %>
  <%= javascript_pack_tag 'application' %>
  </head>
  
  <body>
    <p class="notice"><%= notice %></p>
    <p class="alert"><%= alert %></p>
    <%= yield %>
  </body>
</html>
```

```config/locales/devise.views.ja.yml
ja:
  activerecord:
    attributes:
      user:
        current_password: 'password'
        email: "tky@gmail.com"
        password: "password"
        password_confirmation: "password"
        remember_me: "REMEMBERME"
    models:
      user: "user"
    errors:
      models:
        user:
          attributes:
            email:
              blank: ""
              taken: ""
            password: 
              blank: ""
              too_short: ""
            password_confirmation:
              confirmation: ""
            current_password:
              blank: ""
devise:
  confirmations:
    new:
      resend_confirmation_instructions: ""
    mailer:
      confirmation_instructions:
        action: ""
        greeting: ""
        instruction: ""
      reset_password_instructions:
        action: ""
        greeting: ""
        instruction: ""
        instruction_2: ""
        instruction_3: ""
      unlock_instructions:
        action: ""
        greeting: ""
        instruction: ""
        message: ""
    passwords:
      edit:
        change_my_password: "
        change_your_password: ""
        confirm_new_password: ""
        new_password: ""
      new:
        forgot_your_password: ""
        send_me_reset_password_instructions: ""
    registrations:
      edit:
        are_you_sure: ""
        cancel_my_account: ""
        currently_waiting_confirmation_for_email: ""
        leave_blank_if_you_don_t_want_to_change_it: ""
        title: ""
        unhappy: ""
        update: ""
        we_need_your_current_password_to_confirm_your_changes: ""
      new:
        sign_up: ""
    sessions:
      sign_in: "LOGIN"
    shared:
      links:
        back: "BACK"
        didn_t_receive_confirmation_instructions: ""
        didn_t_receive_unlock_instructions: ""
        forgot_your_password: ""
        sign_in: ""
        sign_in_with_provider: ""
        sign_up: ""
      unlocks:
        new:
          resend_unlock_instructions: ""
```

```config/application.rb
config.time_zone = 'Asia/Tokyo'
config.i18n.default_locale = :ja
```

```app/views/layouts/application.html.erb
<!DOCTYPE html>
<html>
  <head>
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>
    
    <%= stylesheet_link_tag 'application', media: 'all' %>
    <%= javascript_pack_tag 'application' %>
  </head>
  
  <body>
    <nav>
      <% if user_signed_in? %>
        <strong></strong>
        <%= link_to 'EDIT', edit_user_registration_path %>
        <%= link_to 'LOGOUT', destroy_user_session_path, method: :delete%>
      <% else %>
        <%= link_to 'NEW REGISTRATION', new_user_registration_path %>
        <%= link_to 'LOGIN', new_user_session_path %>
        <% end %>
      <% end %>
    </nav>
    
    <p class="notice"><%= notice %></p>
    <p class="alert"><%= alert %></p>
    
    <%= yield%>
  </body>
</html>
```

```app/models/user.rb
class User < ApplicationRecord
  devise :database_authenticable, :registerable,
    :recoverable, :rememberable, :validable,
    :confirmable, :lockable, :timeoutable, :trackable
end
```

```db/migrate/[timestamps]_devise_create_users.rb
class DeviseCreateUsers < ActiveRecord::Migration[6.0]
  def change
    t.string :email, null:false, default: ""
    t.string :encrypted_password, null: false, default: ""
    
    t.string :reset_password_token
    t.datetime :reset_password_sent_at
    
    t.datetime :remember_created_at
    
    t.integer :sign_in_count, default: 0, null: false
    t.datetime :current_sign_in_at
    t.datetime :last_sign_in_at
    t.string :current_sign_in_ip
    t.string :last_sign_in_ip
    
    t.string :confirmation_token
    t.datetime :confirmed_at
    t.datetime :confirmation_sent_at
    t.string :unconfirmed_email
    
    t.integer :failed_attempts, default: 0, null: false
    t.string :unlock_token
    t.datetime :locked_at
    
    t.timestamps null: false
  end
  
  add_index :users, :email, unique: true
  add_index :users, :reset_password_token, unique: true
  add_index :users, :confirmation_token, unique: true
  add_index :users, :unlock_token, unique: true
end
```

```config/initializers/devise.rb
config.mailer_sender = 'info@gmail.com'

config.unlock_strategy = :email
config.maximum_attempts = 3
config.timeout_in = 30.minutes
```

```config/environments/development.rb
config.action_mailer.default_url_options = {}
config.action_mailer.raise_delivery_errors = true
config.action_mailer.delivery_method = "smtp
config.action_mailer.smtp_settings = {
  :address => "smtp.ex.com",
  :port => 465,
  :user_name => "user",
  :password => "password",
  :authentication => :plain,
  :ssl => true,
  :tls => true,
  :enable_starttls_auto => true
}
```

```app/views/devise/mailer/confirmation_instructions.html.erb
<p><%= t('.greeting', recipient: @email) %></p>
<p><%= t('.instruction') %></p>
<p><%= link_to t('.action'), confirmation_url(@resource, confirmation_token: @token) %></p>

<p><%= t('.greeting', recipient: @email)%></p>
<p><%= t('.instruction') %></p>
<p><%= link_to t('.action'), confirmation_url(@resource, confirmation_token: @token) %></p>
<p><%= confirmation_url(@resource, confirmation_token: @token) %></p>

```

######
```
```

```
```

