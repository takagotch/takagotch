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

```
```


```
```

```
```

```
```



