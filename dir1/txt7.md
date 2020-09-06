###### SSL HTTPS 
---


```app/controllers/application_controller.rb
class ApplicationController < ActionController::Base
  include SslRequirement
  
end



```

```config/environment.rb
config.to_prepare do
  SessionsController.ssl_required :new, :create
  RegistrationsController.ssl_required :new, :create
  
  Devise::SessionController.ssl_required :new, :create
end

```

```config/environments/production.rb
config.to_prepare { Devise::SessionsController.force_ssl }
config.to_prepare { Devise::RegistrationsController.force_ssl }
config.to_prepare { Devise::PasswordsController.force_ssl }

```

```config/environments/production.rb
config.action_mailer.default_url_options = { protocol: 'https', :host => 'YOUR_HOST' }
```

```
```

###### HTTP Basic Authentication

```initializers/user.rb

def http_authenticate
  authenticate_or_request_with_http_digest do |user_name, password|
    user_name == "tky" && password == "xxx"
  end
  warden.custom_failure! if performed?
end

```

###### Remote authentication, devise

```user.rb
class User
  attr_accessor :id
  
  #
  include ActiveModel::Validations
  extend ActiveModel::Callbacks
  extend Devise::Models
  
  #
  devise :remote_authenticatable
  
end

```

```remote_authentication.rb
module Devise
  module Models
    module RemoteAuthenticatable
      extend ActiveSupport::Concern
      
      #
      def remote_authentication(authentication_hash)
      end
      
      # serialize
      module ClassMethods
        def serialize_from_session(id)
          resource = self.new
          resource.id = id
          resource
        end
        
        def serialize_into_session(record)
          [record.id]
        end
        
      
    end
  end
end

```

```remote_authenticable.rb
module Devise
  module Strategies
    class RemoteAuthenticatable < Authenticatable
      def authenticate!
        auth_params = authentication_hash
        auth_params[:password] = password
        
        resource = mapping.to.new
        
        return fail! unless resource
        
        if validate(resource) { resource.remote_authentication(auth_params) }
          success!(resource)
        end
      end
    end
    
  end
end

```

```config/initializers/devise.rb
config.warden do |manager|
  manager.strategies.add(:remote, Devise::Strategies::RemoteAuthenticatable)
  manager.default_strategies(:scope => :user).unshift :remote
end

```

###### sign out, rememberable, omniauthable

```.rb
devise_scope :user do
  get 'sign_out', :to => 'devise/sessions#destroy', :as => :destroy_user_session
end

```

```.rb
remember_me(@user)
```

```
```

```spec/support/omniauth.rb
OmniAuth.config.test_mode = true

OmniAuth.config.mock_auth[:twitter] = OmniAuth::AuthHash.new({
  :provider => 'twitter',
  :uid => 'xxxx'
})

OmniAuth.config.add_mock(:twitter, {:uid => 'xxxx'})

OmniAuth.config.mock_auth[:twitter] = :invalid_credentials

OmniAuth.config.on_failure = Proc.new { |env|
  OmniAuth::FailureEndpoint.new(env).redirect_to_failure
}

OmniAuth.config.mock_auth[:twitter] = nil

before do
  request.env["devise.mapping"] = Devise.mappings[:user]
  request.env["omniauth.auth"] = OmniAuth.config.mock_auth[:twitter]
end

before do
  Rails.application.env_config["dvise.mapping"] = Devise.mappings[:user]
  Rails.application.env_config["omniauth.auth"] = OmniAuth.config.mock_auth[:twitter]
end


```

```devise.rb
Devise.setup do |config|
  config.omniauth :twitter, ENV['TWITTER_KEY'], ENV['TWITTER_SECRET']
end

```

```omniauth.rb
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :twitter, ENV['TWITTER_KEY'], ENV['TWITTER_SECRET']
end

```

```authentications_controlller.rb
get "/auth/:action/callback", :to => "authentications",
                              :constraints => { :action => /twitter|google/ }
                              
                              
get "/auth/:action/callback", :controller => "authentications",
                              :constraints => { :action => /twitter|google/ }

get "/auth/:provider/callback" => "authentications#create"

devise_scope :user do
  get "/auth/:provider/callback" => "authentications#create"
end

Rails.application.config.middleware.use OmniAuth::Builder do
  on_failure { |env| AuthenticationsController.action(:failure).call(env) }
end

OmniAuth.config.on_failure = Proc.new { |env| AuthenticationController.action(:failure).call(env) }
```

```sh
vi Gemfile
+ gem 'omniauth-facebook'
bundle install


```

```config/initializers/devise.rb
config.omniauth :facebook,
                "APP_ID",
                "APP_SECRET"

config.omniauth :facebook,
                "APP_ID",
                "APP_SECRET",
                callback_url: "CALLBACK_URL"


```

```app/models/user.rb
devise :omniauthable, :omniauth_providers => [:facebook]
```

```app/views/home.html.erb
<%= link_to "Sign in with Facebook", user_facebook_omniauth_authorize_path %>
```

```.rb
devise_for :users, :controllers => { :omniauth_callbacks => "users/omniauth_callbacks" }
```

```app/controllers/users/omniauth_callbacks_controller.rb
class Users::OmniauthCallbacksController < Devise::OmniauthCallbacksController

  def facebook
    @user = User.from_omniauth(request.env["omniauth.auth"])
    
    if @user.persisted?
      sign_in_and_redirect @user, :event => :authentication
      set_flash_message(:notice, :success, :kind => "Facebook") if is_nabigational_format?
    else
      session["devise.facebook_data"] = request.env["omniauth.auth"]
      redirect_to new_user_registration_url
    end
  end
  
  def failure
    redirect_to root_path
  end
end

```

```app/models/user.rb
def self.from_omniauth(auth)
  where(provider: auth.provider, uid: auth.uid).first_or_create do |user|
    user.email = auth.info.email
    user.password = Devise.friendly_token[0,20]
    user.name = auth.info.name
    user.image = auth.info.image
  # user.skip_confirmation!
  end
end

```

```activerecord/lib/active_record/querying.rb
module ActiveRecord
  module Querying
    delegate :first_or_create, :first_or_create!, :first_or_initialize, to: :all
  end
end

```

```user.rb
class User < ApplicationRecord
  def self.new_with_session(params, session)
    super.tap do |user|
      if data = session["devise.facebook_data"] && session["devise.facebook_data"]["extra"]["raw_info"]
        user.email = data["email"] if user.email.blank?
      end
    end
  end
  
end

```

```config/routes.rb
devise_scope :user do
  delete 'sign_out', :to => 'devise/sessions#destroy', :as => :destroy_user_session
end

devise_for :user, :controllers => { :omniauth_callback => "users/omniauth_callbacks" }

devise_scope :user do
  get 'sign_in', :to => 'devise/session#new', :as => :new_user_session
  get 'sign_out', :to => 'devise/sessions#destroy', :as => :destroy_user_session
end

```

```app/controllers/application_controller.rb
class ApplicationController < ActionController::Base
  def new_session_path(scope)
    new_user_session_path
  end
end

```

```.rb
class Users::OmniauthCallbacksController < Devise::OmniauthCallbacksController
  def facebook
    if request.env["omniauth.auth"].info.email.blank?
      redirect_to "/users/auth/facebook?auth_type=rerequest&scope=email"
    end
  end
end

```

```config/routes.rb
config.omniauth :facebook,
                "APP_ID",
                "APP_SECRET",
                scope: 'email',
                info_fields: 'email,name'

config.omniauth :facebook,
                "APP_ID",
                "APP_SECRET",
                :client_options => {:ssl => {:ca_path => '/etc/ssl/certs'}}
                
config.omniauth :facebook,
                "APP_ID",
                "APP_SECRET",
                :client_options => {:ssl => {:ca_file => {:ssl => {:ca_file => '/usr/lib/ssl/certs/ca-certificates.crt'}}}}

config.omniauth :facebook,
                "APP_ID",
                "APP_SECRET",
                :client_options => {:ca_file => '/usr/lib/ssl/carts/ca-certificates.crt'}

require "omniauth-facebook"
config.omniauth :facebook,
                "",
                "",
                :client_options => { :ssl => { :verify => }}
```

```
# heroku
/usr/lib/ssl/certs/ca-certificates.crt

# engine yard cloud servers
/etc/ssl/certs/ca-certificates.crt
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```
