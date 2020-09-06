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
  
  before_action :one_user_registered?, only: [:new, :create]
  
  def one_user_registered?
    if User.count == 1
      if user_signed_in?
        redirect_to root_path
      else
        redirect_to new_user_session_path
      end
    end
  end
  
end

```

```app/views/registration.html.erb
<% if user_signed_in? %>
  <%= link_to('logout', destroy_user_session_path, method: :delete) %>
<% else %>
  <%= link_to('login', new_user_session_path) %>
<% end %>
```

```
```

###### guest user create

```app/controllers/application_controller.rb
class ApplicationController < ActionController::Base

  protect_from_forgery
  
  def current_or_guest_user
  end
  
  def guest_user(with_retry = true)
    @cached_user ||= User.find(session[:guest_user_id] ||= create_guest_user.id)
    
  rescue ActiveRecord::RecordNotFound
    session[:guest_user_id] = nil
    guest_user if with_retry
  end
  
  private
  
  def logging_in
    # guest_comments = guest_user.comments.all
    # guest_comments.each do |comment|
    #   comment.user_id = current_user.id
    #   comment.save!
    # end
  end
  
# def create
#   super
#   current_or_guest_user
# end
#
# skip_before_filter :verify_authenticity_token, :only => [:name_of_your_action]
#
# protect_from_forgery :except => :receive_guest
  
  def create_guest_user
    u = User.new(:name => "guest", :email => "guest_#{Time.now.to_i}#{rand(100)}@gmail.com")
    u.save!(:validate => false)
    session[:guest_user_id] = u.id
    u
  end
end
```

```initializers/some_initializer.rb
Warden::Strategies.add(:guest_user) do
  def valid?
    session[:guest_user_id].present?
  end
  
  def authenticate!
    manager.default_strategies(scope: :user).unshift :guest_user
  end
end

```

```initializers/devise.rb
Devise.setup do |config|
  config.warden do |manager|
    manager.default_strategies(scope: :user).unshift :guest_user
  end
end

```

```tests/controller_macros.rb
module ControllerMacros
  
  def login_guest(guest = false)
    guest ||= FactoryGirl.create(:guest_user)
    @request.session[:guest_user_id] = guest.id
  end
end


```

```tests/users.rb
FactoryGirl.define do
  factory :user do
  #
  end

  factory :guest_user, class: 'User' do
    sequence(:email) { Faker::Internet.email }
    role             :guest
    to_create        { |instance| instance.save(validate: false) }
  end
end

```

```tests/page_controller_spec.rb
describe PagesController, type: :controller do
  context '#index' do
    before do
    end
    
    it { expect(response).to have_http_status(:success) }
    it { expect(response).to render_template('pages/index') }
  end
end

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

