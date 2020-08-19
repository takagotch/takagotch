###### rails6 deploy heroku
---



```config/puma.rb
# http://localhost:3000/
if ENV.fetch('RAILS_ENV') { 'development' } == 'development'
  ssl_bind '0.0.0.0', '3000', key: './localhost-key.pem', cert: './localhost.pem'
end


```

```sh
brew install heroku/brew/heroku
brew tap heroku/brew && brew install heroku


heroku apps:create
heroku apps:destroy --app apptky
git remote -v
heroku addons:create cleardb:ignite
heroku config

heroku config:add DB_NAME='dbtky'
heroku config:add DB_USERNAME="tky"
heroku config:add_DB_PASSWORD="password"
heroku config:add_DB_HOSTNAME="dbhost"
heroku config:add DB_PORT="3306"

git push heroku master
heroku run rails db:migrate
heroku apps:info


```

```sh
postgres -D /usr/local/var/postgres
rails new memberlist -d postgresql
cd apptky
rails s

createdb apptky_development
rails g scaffold Member name:string comment:text
rake db:migrate
rails s

git init
git add .
git commit -m '1st'

vi Gemfile
# gem 'rails_12factor', group: :production
bundle install
vi Procfile
git commit -m 'procfile'
heroku create
git remote -v
git push heroku master

heroku addons:add heroku-postgresql
heroku run rake db:migrate
heroku apps:info
```

###### ActiveInteraction
```app/interaction/accounts/create_account.rb
def create
  outcome = Reports::Create.run(reports_create_params)
  
  if outcome.valid?
    redirect_to root_url, notice: 'create!'
  else
    flash.now[:alert] = 'error!'
    render :new
  end
end



```

```app/interactions/create_account.rb
module Reports
  class Create < ActiveInteraction::Base
    object :order, class: Order
    object :account, class: Account
    string :content, default: nil
    array :images, default: []
    array :images, default: []
    string :to_state, default: nil
    
    def execute
      report = order.reports.buld(
        content: content, images: images,
        account: account, date: Time.current.to_date
      )
      
      ActiveRecord::Base.transaction do
        report.save!
        
        unless to_state&.to_sym == :accepted
          compose(Notifications::Send)
        end
      end
    end
    
    report
  end
end
```

```.rb
object :purchase, class: Order
object :account, class: Account
string :content, default: nil
array :images, default: []
```


```config/application.rb
config.autoload_paths += Dir.glob("#{config.root}/app/interactions/*")
```

```callbacks
class AutomaticTransactionInteraction = ActiveInteraction::Base
  def execute
  end
end

class ManualTransactionInteraction < ActiveInteraction::Base
  def execute
    ActiveRecord::Base.transaction do
    #
    raise ActoveRecord::Rollback if invalid?
    end
  end
end

```

``` composition
class AddAndDouble < ActiveInteraction::Base
  import_filter Add
  
  def execute
    compose(Add, inputs) * 2
  end
end
```

###### RailsAdmin
```
vi Gemfile
# gem 'devise'





```

```sh
bundle install
rails g devise:install
rails g devise admin_user
rails db:migrate
vi Gemfile
# gem 'rails_admin'
bundle install
rails g rails_admin:install
rake db:migrate
rails c
```

```app/views/layouts/application.html.erb
<!DOCTYPE html>
<html>
<head>
  <title></title>
  <%= stylesheet_link_tag "application", :media => "all" %>
  <%= javascript_include_tag "application" %>
  <%= csrf_meta_tags %>
</head>
<body>
<p class="notice"><%= notice %></p>
<p class="alert"><%= alert %></p>
<%= yield %>
</body>
</html>
```


```app/models/admin_user.rb
class AdminUser < ActiveRecord::Base
  devise :database_authenticatable, :rememberable, :trackable, :validatable
  
  attr_accessible :email, :password, :password_confirmation, :remember_me
end

```

```db/migrate[timestamps]_devise_create_admin_user.rb
class DeviseCreateAdminUsers < ActiveRecord::Migrate
  def change
    create_table(:admin_users) do |t|
    
    # t.string :reset_password_token
    # t.datatime :reset_password_sent_at
  end
  
  # add_index :admin_users, :reset_password_token, :unique => true
end

```

```callbacks .rb
class Increment < ActiveInteraction::Base
  set_callback :type_check, :before, -> { puts 'before type check' }
  
  integer :x
  
  set_callback :validate, :after, -> { puts 'after validate' }
  
  validates :x,
    numericality: { greater_than_or_equal_to: 0 }
    
  set_callback :execute, :around, lambda { |_interaction, block|
    puts '>>'
    block.call
    puts '<<'
  }
  
  def execute
    puts 'executing'
    x + 1
  end
end

Increment.run(x: 1)

```


```create update  .rb

class UpdateAccount < ActiveInteraction::Base
  object :account
  
  string :first_name, :last_name,
    default: nil
    
  validates :first_name,
    presence: true,
    if: :first_name?
  validates :last_name,
    presence: true,
    if: :last_name?
    
  def execute
    account.first_name = first_name if first_name?
    account.last_name = last_name if last_name?
    
    unless account.save
      errors.merge!(account.errors)
    end
    
    account
  end
end


def update
  inputs = { account: find_account! }.reverse_merge()
  outcome = UpdateAccount.run(inputs)
  
  if outcome.valid?
    redirect_to(outcome.result)
  else
    @account = outcome
    render(:edit)
  end
end

```

```edit destroy.rb
def edit
  account = find_account!
  @account = UpdateAccount.new(
    account: account,
    first_name: account.first_name,
    last_name: account.last_name)
end

def destroy
  DestroyAccount.run!(account: find_account!)
  redirect_to(accounts_url)
end

def create
  outcome = CreateAccount.run(params.fetch(:account, {}))
  
  if outcome.valid?
    redirect_to(outcome.result)
  else
    @account = outcome
    render(:new)
  end
end

```

```new show index .rb
def index
  @accounts = ListAccounts.run!
end

class ListAccounts < ActiveInteraction::Base
  def execute
    Account.not_deleted.order(last_name: :asc, first_name: :asc)
  end
end

def show
  @account = find_account!
end

def find_account!
  outcome = FindAccount.run(params)
  
  if outcome.valid?
    outcome.result
  else
    fail ActiveRecord::RecordNotFound, outcome.errors.full_messages.to_sentence
  end
end

class FindAccount < ActiveInteraction::Base
  integer :id
  
  def execute
    account = Account.not_deleted.find_by_id(id)
    
    if account
      account
    else
      errors.add(:id, 'does not exist')
    end
  end
end

def new
  @account = CreateAccount.new
end

class CreateAccount < ActiveInteraction::Base
  string :first_name, :last_name
  
  validates :first_name, :last_name,
    presence: true
    
  def to_model
    Account.new
  end
  
  def execute
    account = Account.new(inputs)
    
    unless account.save
      errors.merge!(account.errors)
    end
    
    account
  end
end
```

```config/application.rb
config.autoload_paths += Dir.glob("#{config.root}/app/interactions/")

```

