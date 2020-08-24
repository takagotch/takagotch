###### CarryWave
---

```app/uploaders/avatar_uploader.rb
class AvatarUploader < CarrierWave::uploader::Base
  storage :file
end

uploader = Avatarloader.new
uploader.store!(my_file)
uploader.retrieve_from_store!('my_file.png')

class User < ActiveRecord::Base
  mount_uploader :avatar, AvatarUploader
end

u = User.new
u.avatar = params[:file] 

File.open('somewhere') do |f|
  u.avatar = f
end

u.save!
u.avatar.url          # => '/url/to/file.png'
u.avatar.current_path # => 'path/to/file.png'
u.avatar_identifier   # => 'file.png'

```


```sh
rails g migration add_avatar_to_users avatar:string
rails db:migrate
```

```
```

###### anyenv

```sh
su -
sudo yum install -y anyenv
anyenv init

anyenv install nodenv
nodenv install -list
nodenv install -l | grep -E '^1\d'
nodenv install $(nodenv install -l | grep -E '^12' | grep -v dev | tail -1)

nodenv global 12.16.3
nodenv global $(nodenv versions | tail -1 | tr '*' ' ' | awk '{print $1}')
nodenv local 12.16.1

anyenv install rbenv
rbenv install $(rbenv install -l | grep -E '^2' | grep -v dev | tail -1)
rbenv global $(rbenv versions | tail -1 | tr '*' ' ' | awk '{print $1}' )
gem install bundler
```

###### acl9
```access.rb
class Admin::SchoolsController < ApplicationController
  access_control do
    allow :support, :of => School
    allow :admins, :managers, :teachers, :of => :school
    deny :teachers, :only => :destroy
    
    action :index do
      allow anonymous, logged_in
    end
    
    allow logged_in, :only => :show
    deny :students
  end
  
  def index
  end
end

```

```config/initializers/acl9.rb
Acl9.config.default_association_name = :roles

Acl9.configure do |c|
  c.default_association_name = :roles
end

Acl9.config.reset!
```


######
---

```
```


```
```

```
```

######
---

```
```


```
```

```
```

######
---

```
```


```
```

```
```

###### blazer
---

```sh
rails g blazer:install
rails db:migrate
```


```config/routes.rb
mount Blazer::Engine, at: "blazer"

ENV["BLAZER_DATABASE_URL"] = "postgres://user:password@hostname:5432/database"
```

```config/environments/production.rb
config.action_mailer.default_url_options = {host: "tkgcci.com"}
```

---

```
# Basic Authentication
ENV["BLAZER_USERNAME"] = "tky"
ENV["BLAZER_PASSWORD"] = "secret"
# Devise
authenticate :user, ->(user) { user.admin? } do
  mount Blazer::Engine, at: "blazer"
end

```


```Encrypto_data.rb
Blazer.transform_variable = lambda do |name, value|
  value = User.generate_email_bidx(value) if name == "email_bidx"
  value
end

```

```pg.yml
data_sources:
  my_source:
    url: postgres://user:password@hostname:5432/database
```
