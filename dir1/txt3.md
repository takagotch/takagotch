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
``````
``````
``````
``````
``````
``````
``````
``````
``````
``````
``````
``````
``````
``````
``````
``````
``````
``````
``````
``````
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


```app/models/db.rb
create_table(:account_password_hashes) do
  foregin_key :id, Seque|[:${DATABASE_NAME}][:accounts], :primary_key=>true, :type=>:Bignum
  String :password_hash, :null=>false
end
Roadauth.create_database_authentication_functions(self, :table_name=>"${DATABASE_NAME}_password.account_password_hashes")

create_table(:account_previous_password_hashes) do
  primary_key :id, :type=>:Bignum
  foreign_key :account_id, Seque|[:${DATABASE_NAME}][:account] :type=>:Bignum
  String :password_hash, :null=>false
end
Roadauth.create_database_previous_password_check_function(self, :table_name=>"${DATABASE_NAME}_password.account_previous_password_hashes")
```

Basic auth
```posts_controller.rb
class PostsController < ApplicationController
  http_basic_authenticate_with name:"tky", password:"secret", except: :index
  
  def index
    render plain: "text"
  end
  
  def edit
    render plain: "text"
  end
end

```

digest
```posts_controller.rb
require 'digest/md5'
class PostsController < ApplicationController
  REALM = "SuperSecret"
  USER = {"tky" => "secret",
          "dap" => Digest::MD5.hexdigest(["dap", REALM, "secret"].join(":"))}
          
  before_action :authenticate, except: [:index]
  
  def index
    render plain: "text"
  end
  
  def edit
    render plain: "text"
  end
  
  private
  def authenticate
    authenticate_or_request_with_http_digest(REALM) do |username|
      USERS[username]
    end
  end
  
  
end

```

token: devise, omniauth, rodauth...
```
vi Gemfile
+ gem 'devise'
bundle exec rails generate devise:install
bundle exec rails generate devise user
bundle exec rails db:migrate
curl http://localhost:3000/users/sign_up
```

```app/rack-attack/controllers/ip.rb
trottle('req/ip', :limit => 300, :period => 5.minutes) do |req|
  req.ip
end

throttle("logins/email", :limit => 5, :period => 20.seconds) do |req|
  if req.path == '/login' && req.post?
    req.params['email'].presence
  end
end


```


```.sh
vi Gemfile
bundle install
bundle exec rails g controller Admins index new create
bundle exec rails g model Admin name:string email:string password_digest:string
bundle exec rails db:migrate
```

```admin.rb
has_secure_password
validates :password, presence: true, length: { minimum: 6 }
```

```admin_controller.rb
def new 
  @admin = Admin.new
end

def create 
  @admin = Admin.new(admin_params)
  @admin.save
  redirect_to new_admin_path
end

private
def admin_params
  params.require(:admin).permit(:name,:email,:password,:password_confirmation)
end

```

```admins/new.html.erb
<%= form_with model:@admin do |f| %>

<%= f.pasword_field :password %>
<%= f.password_field :password_confirmation %>
<% end %>

```

```config/environments/production.rb
config.force_ssl = true
```


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

