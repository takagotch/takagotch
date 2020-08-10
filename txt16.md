######
---

###### sidekiq, delayed_job, resque
```sh
vi Gemfile
# gem 'sidekiq'
vi config/initializers/sidekiq.rb
+ Sidekiq.configure_server do |config|
+  config.redis = { url: 'redis://localhost:6379', namespace: 'sidekiq' }
+ end
vi app/workers/event_worker.rb
+ class EventWorker
+   include Sidekiq::Worker
+   sidekiq_options queue: :event
+ 
+   def perform(id)
+     @event = Event.find(id)
+     @event.calculate_rank!
+   end
+ end
vi app/controllers/events_controller.rb
+ class EventsController
+   def ranking
-     EventWorker.perform_async @event.id
+     EventWorker.perform_in 1.hour, @event.id
+   end
+ end
bundle exec sidekiq -q default event
vi config/sidekiq.yml
+ :verbose: false
+ :pidfile: ./tmp/pids/sidekiq.pid
+ :logfile: ./log/sidekiq.log
+ :concurrency: 25
+ :queues:
+   - default
+   - event
bundle exec sidekiq -C config/sidekiq.yml
vi config/deploy.rb
+ require 'sidekiq/capistrano'
+ set :sidekiq_role, :web
vi app/workers/event_worker.rb
+ sidekiq_options queue: :event, retry: 5
vi app/workers/event_worker.rb
+ sidekiq_options queue: :event, retry: false
vi Gemfile
# 'sinatra', require: false
vi config/routes.rb
+ require 'sidekiq/web'
+ MyApp::Application.routes.draw do
+   mount Sidekiq::Web, at: "/sidekiq"
+ end
vi config/routes.rb
+ require 'sidekiq/web'
+ MyApp::Application.routes.draw do
+   authenticate :user, lambda { |u| u.admin? } do
+     mount Sidekiq::Web => '/sidekiq'
+   end
+ end
vi Gemfile
# group :test do
#   gem 'rspec-sidekiq'
# end
vi spec/workers/event_worker_spec.rb
+ require 'spec_helper'
+ describe EventWorker do
+   let(:test_event) { create(:event) }
+   before do
+     subject.perform test_event.id
+   end
+   it "perform job queue in" do
+     should be_processed_in(:event)
+   end
+ end
```

```rb





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

