###### Rack
---



###### sprockets, webpacker, webpack
```config/webpack/[development|test|production].js
```

```.sh
bundle exec rake webpacker:compile
bundle exec rake assets:precompile

./node_modules/.bin/webpack --config config/webpack/development.js
```

```webpacker/lib/webpacker/webpack_runner.rb
require "shellwords"
require "webpacker/runner"

module Webpacker
  class WebpackerRunner < Webpacker::Runner
    def run
      env = { "NODE_PATH" => @node_modules_path.shellescape }
      cmd = [ "#{node_modules_path}/.bin/webpack", "--config", @webpack_config ] + @argv
      
      Dir.chdir(@app_path) do
        exec env, *cmd
      end
    end
    
    
  end
  
end

```

```webpacker/lib/webpacker/manifest.rb
class Webpacker::Manifest
  class MissingEntryError < StandardError; end
  
  def lookup(name)
    compile if compiling?
    find name
  end
  
  private
  def compiling?
    config.compile? && !dev_server.running?
  end
  
end

```

```webpacker.yml
default: &default
  source_path: app/javascript
  
development:
  <<: *default
  compile: true

dev_server:
  http: false
  host: localhost
  port: 3035
```

```webpacker/lib/webpacker/dev_server.rb
class Webpacker::DevServer
  def running?
    if config.dev_server.present?
      Socket.tcp(host, port, connect_timeout: connect_timeout).close
    else
      false
    end
  rescue
    false
  end
  
  def host
    fetch(:host)
  end
  
  def port
    fetch(:port)
  end
  
  private
    def fetch(key)
      ENV["WEBPACKER_DEV_SERVER_#{key.upcase}"] || config.dev_server.fetch(key, defaults[key])
    end
end

```

```docker-compose.yml
version: '3'

services:
  backend:
    command: 'bundle exec rails s -b 0.0.0.0'
    environment:
      - WEBPACKER_DEV_SERVER_HOST: frontend
  frontend:
    command: './node_modules/.bin/webpack-dev-server --config config/webpack/development --host 0.0.0.0 --port 3035'

```

```config/webpack/environement.js
const { environment } = require('@rails/webpacker')

environment.splitChunks()

environment.splitChunks((config) => Object.assign({}, config, { optimization: { splitChunks: false }}))

module.exports = environment

# v3
const { environment } = require('@rails/webpacker')
const webpack = require('webpack')

environment.plugins.append(
  'CommansChunkVendor',
  new webpack.optimize.CommonsChunkPlugin({
    name: 'vendor',
    minChunks: (module) => {
      return module.context && module.context.indexOf('node_modules') !== -1
    }
  })
)

module.exports = environment
```

```app/views/home.html.erb
<%= javascript_pack_tag 'vendor' %>
<%= javascript_packs_with_chunks_tag 'tky' %>
```

```app/views/layouts/application.html.erb
<%= javascript_pack_tag "vendor" %>
<%= javascript_pack_tag 'entry_file_name' %>
```

###### Rack::Tracker

```config/initilizers/rack-tracker.rb
config = MyApplication::Application.config
config.middleware.use(Rack::Tracker) do
  handler :google_analytics, { tracker: 'U-xxxx-Y' }
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

```spec/support/controller_macros.rb
module ControllerMacros
  def login user(user=nil)
    @request.env["devise.mapping"] = Devise.mappings[:user]
    user ||= FactoryGirl.create(:user)
    sign_in user
  end
end
```

```spec/rails_helper.rb, spec/support/devise.rb
RSpec.configure do |config|
  config.include Devise::Test::ControllerHelpers, :type => :controller
  config.include ControllerMacros, :type => :controller
end

Dir[Rails.root.join('spec/support/**/*.rb')].each { |f| require f }
```

```spec/users/user_controller_helper.rb
require 'rails_helper'

RSpec.describe UsersController, type: :controller do
  describe 'GET #index' do
    context 'with assign all users' do
      before do
        login_user
        get :index
      end
      it { is_expected.to have_http_status :success }
      it { is_expected.to render_template :index }
    end
  end
end

let(:user) { FactoryGirl.create(:user) }
before do
  allow(controller).to receive(:current_user).and_return(user)
end
```

```.sh
./bin/rspec/ spec/controllers/users_controller_spec.rb
```

```.sh
vi Gemfile
+ gem 'devise'
./bin/rails g devise:install

./bin/rails g devise User
./bin/rails db:create db:migrate

./bin/rails g devise:views users

./bin/rake haml:erb2haml

./bin/rails g devise:controllers users

vi Gemfile
+ gem 'devise-i18n'
bundle install

./bin/rails g devise:i18n:views users
./bin/rake haml:erb2haml
./bin/rails g devise:i18n:locale ja
./bin/rails g devise:i18n:locale en

rm -rf config/locales/devise.en.yml

```

```config/environments/development.rb
config.action_mailer.default_url_optios = { host: 'localhost', port: 3000 }
```

```app/views/layouts/application.html.haml
%p.notice = notice
%p.alert = alert
```

```app/models/user.rb
class User < ApplicationRecord
  devise :database_authenticable, :registerable,
         :recoverable, :rememberable, :trackable, :validable
         :lockable, :confirmable, :timeoutable
end
```

```config/initializers/devise.rb
config.scoped_views = true
```

```app/controllers/users/sessions_controller.rb
class Users::SessionsController < Devise::SessionsController
  def create
    super do |resource|
    end
  end
  
end

```

```config/routes.rb
Rails.application.rotes.draw do
  devise_for :users, module: :users

end

```

```config/application.rb
config.i18n.default_locale = :ja
```

```
```

```
```

```
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

