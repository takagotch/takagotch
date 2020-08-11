######
---



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


######
```
```

```
```

