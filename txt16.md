######
---



```sh

```

###### ransack
```sh
rails new search_engine
cd search_engine
vi Gemfile
# gem 'ranrack'
bundle install
rails g scaffold user name age:integer sex:integer address
rails db:migrate
vi app/controllers/users_controller.rb
+ def index
+   @q = User.ransack(params[:q])
+   @users = @q.result(distinct: true)
+ end
vi db/seeds.rb
+ User.create(name: 'tky', age: '21', sex: '1', address: 'kitahama')
+ User.create(name: 'takagotch', age: '23', sex: '2', address: 'umeda')
+ User.create(name: 'tkgcci', age: '25', sex: '1', address: 'namba')
rails db:seed
vi app/views/index.html.erb
+ <%= search_form_for @q do |f|%>
+
-   <%= f.label :name, "KEYWORD" %>
-   <%= f.search_field :name_cont %>
+   <%= f.label :name_or_address, "KEYWORD" %>
+   <%= f.search_field :name_or_address_cont %>
+  
+   <%= f.label :age %>
+   <%= f.number_field :age_gteq %>OVER~
+   <%= f.number_field :age_lt %>UNDER
+
+   <%= f.label :sex %>
+   <%= f.radio_button :sex_eq, '', {:checked => true} %>UNSPECIFIED
+   <%= f.radio_button :sex_eq, 1 %>MALE
+   <%= f.radio_button :sex_eq, 2 %>FEMALE
+
+  <%= f.submit "SEARCH" %>
+ <% end %>
vi app/views/index.html.erb
+ <tr>
+   <th><%= sort_link(@q, :name, "NAME")%></th>
+   <th><%= sort_link(@q, :age, "AGE")%></th>
+   <th><%= sort_link(@q, :sex, "SEX")%></th>
+   <th><%= sort_link(@q, :address, "ADDRESS")%></th>
+   <th colspan="3"></th>
+ </tr>
```

###### kaminari
```sh
vi app/controller/home_controller.rb
+ class HomeController < ApplicationController
+   def top
+     @posts = Post.all.order(created_at: :desc)
+     @posts = Post.page(params[:page]).per(20)
+   end
+ end
vi app/views/_postindex.html.erb
+ <% @posts.each do |post| %>
+ <div class="panel panel-success">
+   <div class="panel-body">
+     <%= link_to(post.content, "/posts/#{post.id}") %>
+   </div>
+   <div class="panel-footer">content_description</div>
+ </div>
+ <% end %>
+ <%= paginate @posts %>
rails g kaminari:view default
vi config
+ Kaminari.configure do |config|
+   config.default_per_page = 25
+   config.max_per_page = nil
+   config.window = 4
+   config.outer_window = 0
+   config.left = 0
+   config.right = 0
+   config.page_method_name = :page
+   config.param_name = :page
+ end
vi app/controller/application.controller.erb
+ class HomeController < ApplicationController
+   def top
+     @posts = Post.page(params[:page]).per(6).order('updated_at DESC')
+   end
+ end
```

###### AcctiveSupport::TimeWithZone
```rb
Time.zone = 'Eastern Time (US & Canada)'
Time.zone.local(2020, 2, 10, 15, 30, 45)
Time.zone.parse('2020-02-10 15:30:45')
Time.zone.at(1171139445)
Time.zone.now
Time.utc(2020, 2, 10, 20, 30, 45).in_time_zone

t = Time.zone.now
t.hour
t.dst?
t.utc_offset
t.zone
t.to_s(:rfc822)
t + 1.day
t.beginning_of_year
t > Time.utc(1999)
t.is_a?(Time)
t.is_a?(ActiveSupport::TimeWithZone)
```

###### jemalloc
```sh
// ubuntu
sudo apt install libjemalloc-dev
su - 
RUBY_CONFIGURE_OPTS=--with-jemalloc rbenv install 2.7.1
rbenv global 2.7.1
sudo systemctl restart apptky-*
sudo systemctl restart apptky-sidekiq.service
systemctl status apptky-sidekiq.serivce


```


```sh

```


```sh

```


```sh

```


```sh

```


```sh

```


```sh

```

