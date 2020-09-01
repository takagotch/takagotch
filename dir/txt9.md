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

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

