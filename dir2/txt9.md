###### ActiveAdmin, Devise
---


```.sh
vi Gemfile
gem 'activeadmin'
gem 'devise'
bundle install

rails g devise:install
rails g active_admin:install


bundle exec rake db:migrate
bundle exec rake db:seed
rails c
bundle exec rails s
curl http://localhost:3000/admin/
rails g active_admin:resource inquiry 

curl http://localhost:3000/admin/
```


```config/initializers/active_admin.rb
config.current_user_method = :current_admin_user
config.current_user_method = :current_user(or current_company)
config.logout_link_path = :destroy_admin_user_session_path
config.logout_link_path = :destroy_user_session_path
config.logout_link_path = :root_path
```

```app/application_controller.rb
def authenticate_admin_user!
  authenticate_user!
  
  unless currrent_user.has_role 'admin'
    flash[:alert] = "LOGIN admin"
    redirect_to root_path
  end
end


```


```app/admin/inquiry.rb
# permit_params :company_name, :name, :email, :phone, :content
permit_param:company_name,:name,:email,:phone,:content

```


```app/models/post.rb
# active_admin has_many
# permit_params :title, :body, images_attributes: [:image, :_destroy, :id]
def Post < ActiveRecord::Base
  has_many :images, dependent: :destroy
  accepts_nested_attributes_for :images, allow_destroy: true
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


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```



