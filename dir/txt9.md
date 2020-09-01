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
class User < ActivreRecord

  devise :omniauthable, omniauth_providers: %i[facebook]
  
end


```

```app/views/home.html.erb

<%= link_to "Sign in with Facebook", user_facebook_omniauth_authorize_path %>

```

```config/routes.rb
devise_for :users, controllers: { omniauth_callbacks: 'users/omniauth_callbacks' }


```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

