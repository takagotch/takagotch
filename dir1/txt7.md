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

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```
