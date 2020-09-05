###### reCAPTCHA Google
---


```recaptcha.html
<html>
<head>
<title></title>
<script src='https://www.google.com/recaptcha/api.js'></script>
</head>
<body>
<div class="g-recaptcha" data-sitekey="MYSITEKEY"></div>
</body>
</html>
```

```application.html.erb
<script src="https://www.google.com/recaptcha/api.js?render=reCAPCHASITEKEY"></script>
<script>
  grecaptcha.ready(function () {
    grecaptcha.execute('reCAPCHASITEKEY', { action: 'contact' }).then(function (token) {
      var recaptchaResponse = document.getElementById('recaptchaResponse');
      recaptchaResponse.value = token;
    });
  });
</script>
```

```sessions/new.html.erb
<% provide(:title, "Log in") %>
<h1></h1>

<div class="row">
  <div class="col-md-6 col-md-offset-3">
    <%= form_for(:session, url: login_path) do |f| %>
      <%= f.label :email %>
      <%= f.email_field :email, class: 'form-control' %>
      
      <%= f.label :password %>
      <%= link_to "(forgot password)", new_password_reset_path %>
      <%= f.password_field :password, class: 'form-control' %>
      
      <%= f.label :remember_me, class: "checkbox inline" do %>
        <%= f.check_box :remember_me %>
        <span>Remember me on this computer</span>
      <% end %>
      <input type="hidden" name="recaptcha_response" id="recaptcharResponse">
      
      <%= f.submit "Log in", class: "btn btn-primary" %>
    <% end %>
    
    <p>New user? <%= link_to "Sign up now!", signup_path %></p>
  </div>
</div>
```

```SessionsController/sessions_controller.rb
require 'uri'
require 'net/http'

class SessionController < ApplicationController

  RECAPTCHA_SECRET_KEY = 'reCAPTCHA CONSOLE SECRETKEY INPUT'
  RECAPTCHA_SITEVERIFY_URL = ''
  
  def create
    siteverify_uri = URI.parse()
    response = Net::HTTP.get_response(siteverify_uri)
    json_response = JSON.parse(response.body)
    
    if json_response['success'] && json_response['score'] > 0.5
      user = User.find_by(email: params[:session][:email])
      if user.activated?
        # SUCCESS
        log_in user
        params[:session][:remember_me] == '1' ? remember(user) : forget(user)
        redirect_back_or user
      else
        message = "Account not activated."
        message += "Check your email for the activation link."
        flash[:warning] = message
        redirect_to root_url
      end
    else
      flash.now[:danger] = 'Invalid email/password combination'
      render 'new'
    end
  end
  
end
```

```
```

```sh
vi Gemfile
+ gem 'recaptcha', require: "recaptcha/rails"
bundle install



```

```config/initializers/recaptcha.rb
Recaptcha.configure do |config|
  config.site_key = ENV['RECAPTCHA_PUBLIC_KEY']
  config.secret_key = ENV['RECAPTCHA_PRIVATE_KEY']
end
```

```app/views/devise/registrations/new.html.erb
<%= recaptcha_tags %>
```

```app/controllers/reistrations_controller.rb
class RegistrationsController < Devise::RegistrationsController
  prepend_before_action :check_captcha, only: [:create]
  prepend_before_action :customize_sign_up_params, only: [:create]
  
  private
  def customize_sign_up_params
    devise_parameter_sanitizer.permit :sign_up, keys: [:username, :email, :password, :password_confirmation, :remember_me]
  end
  
  def check_captcha
    self.resource = resource_class.new sign_up_params
    resource.validate
    unless verify_recaptcha(model: resource)
      respond_with_navigational(resource) { render :new }
    end
  end
end
```

```config/locales/ja.yml
recaptcha:
  errors:
    verification_failed: 'reCAPTCHA ERR'
```

```
```

###### devise, single user

```config/routes.rb
devise_for :users, controllers: { registrations: "registrations" }
```

```app/controllers/registrations_controller.rb
class RegistrationsController < Devise::RegistrationsController
end

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


###### reCAPTCHA, Devise

```.sh
vi Gemfile
+ gem 'devise'
bundle install

rails g devise:install
rails g devise user
rake db:migrate
rails g devise:views
rails g devise:controllers users


```

```config/routes.rb
devise_for :users, :controllers => {
  :registrations => 'users/registrations',
  :sesssions => 'users/sessions'
}

devise_scope :user do
  get "sign_in", :to => "users/sessions#new"
  get "sign_out", :to => "users/sessions#destroy"
end

```

```application.html.erb


```

```application_controller.rb

# => {""=>true, ""=>"", ""=>"",...}
```

```app/controllers/users/session_controller.rb
class Users::SessionsController < Devise::SessionController
  
  # sign_in
  def create
    unless verify_recaptcha?(params[:recaptcha_token], 'login')
      flash.now[] = I18n.t('recaptcha.errors.verification_failed') # ERR i18n
      return render action: :new
    end
    self.resource = warden.authenticate!(auth_options)
    sign_in(resource_name, resource)
    yield resource if block_given?
    respond_with resource, location: after_sign_in_path_for(resource)
  end
  
  protected
  
  def sign_in_params
    devise_parameter_sanitizer.sanitize(:sign_in)
  end
  
  def auth_options
    { scope: resource_name, recall: "#{controller_path}#new" }
  end
  
  def translation_scope
    'devise.sessions'
  end
  
  # sign_out
  
end


```

```app/views/users/sessions/new.html.erb


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

