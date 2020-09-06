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

```spec/support/omniauth.rb
OmniAuth.config.test_mode = true

OmniAuth.config.mock_auth[:twitter] = OmniAuth::AuthHash.new({
  :provider => 'twitter',
  :uid => 'xxxx'
})

OmniAuth.config.add_mock(:twitter, {:uid => 'xxxx'})

OmniAuth.config.mock_auth[:twitter] = :invalid_credentials



```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```
