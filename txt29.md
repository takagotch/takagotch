######
---

```
```


```
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


