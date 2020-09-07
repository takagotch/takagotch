###### Rack
---



###### sprockets
```
```

```
```

```
```

```
```

```
```

###### Rack::Tracker
```
```
###### Unicorn jtt[

```
```
###### OmniAuth

```config/initializers/omniauth.rb
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :twitter, ENV['TWITTER_KEY'], ENV['TWITTER_SECRET']
end

```

```.sh
rake middleware

```

```lib/omniauth/strategy.rb

def setup_phase
  if options[:setup].respond_to?(:call)
    log :info, 'Setup endpoint detected, running now.'
    options[:setup].call(env)
  elsif options.setup?
    log :info, 'Calling through to underlying application for setup.'
    setup_env = env.merge('PATH_INFO' => setup_path, 'REQUEST_METHOD' => 'GET')
    call_app!(setup_env)
  end
end

module OmniAuth
  module Strategies
    class Developer
      include OmniAuth::Strategy
      
      options :fields, [:name, :email]
      options :uid_field, :email
    end
  end
end

app = lambda{|env| [200, {}, ["Hello."]]}
OmniAuth::Strategies::Developer.new(app).options.uid_field                             # => :email
OmniAuth::Strategies::Developer.new(app, :uid_field => :name)options.uid_field # => :name


```

```lib/omniauth/callback.rb
module OmniAuth
  module Strategies
    class Developer
      include OmniAuth::Strategy
      
      uid do
        request.params[options.uid_field.to_s]
      end
    end
  end
  
end

```

```config/initializers/omniauth.rb
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :facebook, ENV['FACEBOOK_KEY'], ENV['FACEBOOK_SECRET'], scope: [:email]
end

```

```lib/omniauth/strategies/oauth2.rb
def request_phase
  redirect client.auth_code.authorize_url({:redirect_uri = callback_url}).merge(authorize_params)
end

```

```lib/omniauth/strategies/oauth2.rb
def client
  ::OAuth2::Client.new(opitons.client_id, options.client_secret, deep_symbolize(options.client_options))
end

```

```lib/omniauth/strategy.rb
def callback_url
  full_host + script_name + callback_path + query_string
end
```

```lib/omniauth/strategies/oauth2.rb
def callback_url
  full_host + script_name + callback_path
end

def build_access_token
  verifier = request.params["code"]
  client.auth_code.get_token(verifier, {:redirect_uri => callback_url}.merge(token_params.to_hash(::symbolize_keys => true)), deep_symbolize(options.auth_token_params))
end

credentials do
  hash = {"token" => access_token.token}
  hash.merge!("refresh_token" => ) if access_token.expires? && access_token.refresh_token
  hash.merge!("expires_at" => access_token.expires_at) if access_token.expires?
  hash.merge!("expires" => access_token.expires?)
  hash
end

```

```.rb
OmniAuth.config.add_camelization('oauth', 'OAuth')

provider :twitter, CLIENT_ID1, CLIENT_SECRET1
provider :twitter, CLIENT_ID2, CLIENT_SECRET2, name: 'twitter2'

client_options: { ssl: { verify: false } }

#
# https://github.com/omniauth/omniauth/wiki/Integration-Testing
```

```
```


###### Rodauth

```
```
###### warden
###### redis-store, Rack, Rack::Cache

###### Coverband

###### Incoming

###### Airbrake

###### Better Errors

###### Bugsnag

###### Exception Notification, rack, rails

###### Split

###### rack-secure-upload

###### Faraday, Net::HTTP

###### Neo4j.rb 

###### Derailed Benchmarks

###### rack-mini-profiler

###### Rack::Attack

###### Rack::Protection

###### Hobbit

###### Rack::App
```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

