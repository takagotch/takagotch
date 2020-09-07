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

```
```
```
```
```
```
```
```
```
```
```
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

