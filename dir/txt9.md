###### omniauth devise
---


```sh
vi Gemfile
+ gem 'omniauth-facebook'
rails g migration AddOmniauthToUsers provider:string uid:string
rake db:migrate
```

```config/initializers/devise.rb
config.omniauth :facebook, "APP_ID", "APP_SECRET", token_params: { parse: :json }
```

```app/models/user.rb
class User < ApplicationRecord
  devise :omniauthable, omniauth_providers: %i[facebook]
  
  def self.from_omniauth(auth)
    where(provider: auth.provider, uid: auth.uid).first_or_create do |user|
      user.email = auth.info.email
      user.password = Devise.friendly_token[0, 20]
      user.name = auth.info.name
      user.image = auth.info.image
      # user_skip_confirmation!
    end
  end
  
  def self.new_with_session(params, session)
    super.tap do |user|
      if data = session["devise.facebook_data"] && session["devise.facebook_data"]["extra"]["raw_info"]
        user.email = data["email"] if user.email.blank?
      end
    end
  end
  
end


```

```app/views/home.html.erb

<%= link_to "Sign in with Facebook", user_facebook_omniauth_authorize_path %>

```

```config/routes.rb
devise_for :users, controllers: { omniauth_callbacks: 'users/omniauth_callbacks' }

devise_scope :user do
  delete 'sign_out', :to => 'devise/sessions#destroy', :as => :destroy_user_session

  get 'sign_in', :to => 'devise/sessions#new', :as => :new_user_session
  get 'sign_out', :to => 'devise/session#destroy', :as => :destroy_user_session
end

config.omniauth :facebook, "APP_ID", "APP_SECRET", scope: 'email', info_fields: 'email,name'

config.omniauth :facebook, ENV['FB_APP_ID'], ENV['FB_APP_SECRET'],
                scope: 'public_profile,email',
                info_fields: 'email,first_name,last_name,gender,birthday,localtion,picture',
                client_options: {
                  site: 'https://graph.facebook.com/v2.11',
                  authorize_url: "https://www.facebook.com/v2.11/dialog/oauth"
                }

OpenSSL::SSL::SSLError (SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed)

config.omniauth :facebook, "APP_ID", "APP_SECRET",
          client_options: { ssl: { ca_path: '/etc/ssl/certs' } }

reqire "omniauth-facebook"
config.omniauth :facebook, "APP_ID", "APP_SECRET", client_options: { ssl: { verify: !Rails.env.development? } }

config.omniauth :facebook, "APP_ID", "APP_SECRET", :strategy_class => OmniAuth::Strategies::Facebook
```

```ca_file
config.omniauth :facebook, "APP_ID", "APP_SECRET",
          client_options: { ssl: { ca_file: '/usr/lib/ssl/certs/ca-certificates.crt' }}
```

```app/controllers/users/omniauth_calbacks_controller.rb
class Users::OmniauthCallbacksController < Devise::OmniauthCallbacksController
  def facebook
    @user = User.from_omniauth(request.env["omniauth.auth"])
    
    if @user.persisted?
      sign_in_and_redirect @user, event: :authentication
      set_flash_message(:notice, :success, kind: "Facebook") if is_navigation_format?
    else
      session[] = request.env["omniauth.auth"].except(:extra)
      redirect_to new_user_registration_url
    end
  end
  
  def failure
    redirect_to root_path
  end
end


```

```app/controllers/application_controller.rb
class ApplicationController < ActionController::Base


  def new_session_path(scope)
    new_user_session_path
  end
  
end


```

```app/controllers/users/callback_controller.rb
class Users::OmniauthCallbacksController < Devise::OmniauthCallbacksController
  def facebook
    if request.env["omniauth.auth"].info.email.blank?
      redirect_to "/users/auth/facebook?auth_type=rerequest&scope=email"
      return
    end
  end
end

```

###### devise_token_auth logout

```sh
rails routes

curl http://localhost:3000/users/sign_out
curl -X DELETE -H 'client:xxxxxxx' -H 'uid-xxxx' -H 'access-token:xxx' http://localhost:3000/users/sign_out | jq .
curl -X DELETE -h 'client:xxxx' -H 'uid:xxxx' -H 'access-token:xxxx' http://localhost:3000/users/sign_out | jq .
```

```config/routes.rb
Rails.application.routes.draw do
  mount_devise_token_auth_for 'User', at: 'auth', controllers: {
    omniauth_callbacks: "users/omniauth_callbacks"
  }
  as :user do
    delete '/users/sign_out', to: 'users/session#destroy', as: :destroy_user_session
  end
  
  mount ApplicationAPI, at: "/"
end

```

```app/controllers/users/sessions_controller.rb
module Users
  class SessionController < DeviseTokenAuth::ApplicationController
    def render_destory_error
      render json: {
        message: I18n.t("devise_token_auth.sessions.user_not_found")
      }, status: 404
    end
  end
end


```

###### devise_token_auth LOGIN

```sh
rails new User -d postgresql -B -T --api
vi Gemfile
+ gem 'rack-cors'
+ gem 'devise'
+ gem 'devise_token_auth'
+ gem 'omniauth'
+ gem 'omniauth-oauth2'
+ gem 'omniauth-google-oauth2'
+ gem 'omniauth-twitter'

rails g devise_token_auth:install User auth
rails db:migrate

rails routes
```

```db/migrate/[timestamps]_devise_token_auth_create_users.rb
class DeviseTokenAuthCreateUsers < ActiveRecord::Migration[5.1]
  def change
    create_table(:user) do |t|
    
      t.string :provider, :null => false, :default => "email"
      t.string :uid, :null => false, :default => ""
    
      t.string :encrypted_password, :null => false, :default => ""
    
    # t.string :reset_password_token
    # t.datetime :reset_password_send_at
  
      t.datetime :remember_created_at
    
      t.integer :sign_in_count, :default => 0, :null => false
      t.datetime :current_sign_in_at
      t.datetiem :last_sign_in_at
      t.string :curent_sign_in_ip
      t.string :last_sign_in_ip
    
    # t.string :confirmation_token
    # t.datetime :confirmed_at
    # t.datetime :confirmation_sent_at
    # t.string :unconfirmed_email 
  
    # t.integer :failed_attempts, :default => 0, :null => false
    # t.string  :unlock_token
    # t.datetime :locked_at
  
      t.string :name
      t.string :nickname
      t.string :image
      t.string :email
  
      t.json :token
  
      t.timestamps
    end
  
    add_index :users, :email, unique: true
    add_index :users, [:uid, :provider], unique: true
  # add_index :users, :reset_password_token, unique: true
  # add_index :users, :confirmtion_token,    unique: true
  # add_index :users, :unlock_token,         unique: true
  end
end

```

```app/models/user.rb
class User < ActiveRecord::Base
  devise :rememberable, :omniauthable
  include DeviseTokenAuth::Concerns::Users
end


```

```app/controllers/users/omniauth_callbacks_controller.rb
module Users
  class OmniauthCallbacksController < DeviseTokenAuth::OmniauthCallbacksContller
    include Devise::Controllers::Rememberable
    
    def omniauth_success
      get_resource_from_auth_hash
      create_token_info
      set_token_on_resource
      create_auth_params
      
    # if resource_class.devise_modules.includes?(:confirmable)
    #   @resource.skip_confirmation!
    # end
    
      sign_in(:user, @resource, store:false, bypass: false)
      
      if @resource.save!
        update_auth_header
        yield @resource if block_given?
        render json: @resource, status: :ok
      else
        render json: { message: "failed to login" }, status: 500
      end
     
    # @resource.save!
    # update_auth_header
    # yield @resource if block_given?
    # render_data_or_redirect('deliverCredentials', @auth_params.as_json, @resource.as_json)
    
    end
    
    protected
    def get_resource_from_auth_hash
      @resource = resource_class.where({
        uid:      auth_hash['uid'],
        provider: auth_hash['provider']
      }).first_or_initialize
      
      if @resource.new_record?
        @oauth_registration = true
      # set_random_password
      end
      
      assign_provider_attrs(@resource, auth_hash)
      
      extra_params = whitelisted_params
      @resource.assign_attributes(extra_params) if extra_params
      
      @resource
      
    end
  
  end
  
end

```

```config/routes.rb
Rails.application.routes.draw do
  mount_devise_token_auth_for 'User', at: 'auth', controllers: { omniauth_callbacks: "users/omniauth_callbacks" }
end

```

```config/initializers/devise_token_auth.rb
DeviseTokenAuth.setup do |config|
  config.change_headers_on_each_request = false
  config.token_lifespan = 1.month
  config.headers_names = {:'access-token' => 'access-token',
                          : 'client' => 'client',
                          : 'expiry' => 'expiry',
                          : 'uid' => 'uid',
                          : 'token-type' => 'token-type' } 
end

```


```config/application.rb
module User
  class Application < Rails::Application
    config.load_defaults 5.1
    config.api_only = true
    
    config.middleware.use Rack::MethodOverride
    config.middleware.use ActionDispatch::Cookies
    config.middleware.use ActionDispatch::Session::CookiesStore
    config.middleware.use ActionDispatch::Flash
    
    config.middleware.insert_before 0, Rack::Cors do
      allow do
        origins '*'
        resource '*',
                  :headers => :any,
                  :expose  => ['access-token', 'expiry', 'token-type', 'uid', 'client'],
                  :methods => [:get, :post, :options, :delete, :put]
      end
    end
  end
  
end

```

```config/initializer/omniauth.rb
require File.expand_path()
Rails.application.config.middleware.use OmniAuth::Builder do
  OAUTH_CONFIG = YAML.load_file("")[Rails.env].symbolize_keys!
  provider :doorkeeper, OAUTH_CONFIG[][], OAUTH_CONFIG[]
  
  provider :google_oauth2, OAUTH_CONFIG[][], OAUTH_CONFIG[:google]['secret'], name: :google, scope: %w(email)
  
  provider :twitter, OAUTH_CONFIG[][], OAUTH_CONFIG[:twitter][]

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

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

