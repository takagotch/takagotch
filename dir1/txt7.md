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

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```
