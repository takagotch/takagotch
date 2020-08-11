######
---

###### sidekiq, delayed_job, resque | ActiveJob
[sidekiq](https://github.com/mperham/sidekiq/wiki)

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
bin/rails g job generate_ticket
vi app/jobs/application_job.rb
# class ApplicationJob < ActiveJob::Base
# end
vi app/jobs/generate_ticket_job.rb
# class GenerateTicketJob < ApplicationJob
#   queue_as :default
#   def perform(*args)
#     p "Hello Active Job."
#   end
# end
// call
# GenerateTicketJob.perform_later
# GenerateTicketJob.perform_later some_arg
# GenerateTicketJob.set(wait: 5.second).perform_later

bundle exec sidekiq

sudo apt-get -y update && sudo apt-get install -y redis-server
sudo systemctl start redis-server
sudo systemctl status redis-server
sudo systemctl enable redis-server
cat app/Gemfile
# gem 'sidekiq'
bundle install
bundel exec sidekiq

vi config/application.rb
+ module ApptkyApp
+   class Application < Rails::Application
+     config.active_job.queue_adapter = :sidekiq
+   end
+ end
rails c --sandbox
require 'sidekiq/api'
Sidekiq::Stats.new.processed
Sidekiq::Stats.new.processed
vi app/jobs/generate_ticket_job.rb
+ class GenerateTicketJob < ApplicationJob
+   queue_as :default
+   def perform
+     ticket = Ticket.create!(name: 'xxx')
+     p "Generating ticket #{ticket.name} ..."
+   end
+ end
vi app/jobs/trashable_clean_job.rb
+ class TrashableCleanupJob < ApplicationJob
+   def perform(trashable, depth)
+     trashable.cleanup(depth)
+   end
+ end

# GenerateTicketJob.new.perform


Sidekiq::Stats.new.to_json
p Sidekiq::Stats.new.to_json
Sidekiq::RetrySet.new.each {|job| puts "#{job.jid} #{job.klass} #{job.args}"}
Sidekiq::RetrySet.new.find_job('xxx')
Sidekiq::RetrySet.new.each {|job| p job }
Sidekiq::RetrySet.new.find_job(<JID>).delete
Sidekiq::RetrySet.new.find_job('xxx').delete
Sidekiq::RetrySet.new.clear
Sidekiq::RetrySet.new.clear
curl http://localhost:3000/sidekiq/
vi config/routes.rb
# require 'sidekiq/web'
# mount Sidekiq::Web => '/sidekiq'
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

###### centos mariadb
```sh
sudo systemctl stop mariadb
sudo systemctl disable mariadb
sudo yum remove mariadb

sudo cd /var/lib/mysql
sudo rm -rf mysql

curl -sS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash
sudo yum install MariaDB-server

sudo vi /etc/my.cnf.d/server.cnf
//
[mariadb]
character-set-server=utf8mbr4
[client-mariadb]
default-character-set=utf8mb4 // emoji utf8mb4

sudo systemctl enable mariadb
sudo systemctl start mariadb
mysql

sudo mysql_secure_installation
Enter
n
y
y
y
y
y
mysql -u root -p
show databases;
exit

sudo yum -y install MariaDB-shared
```

###### Vuex
```sh
cd ~/
mkdir tkyapp && cd tkyapp
rails new . --webpack=vue --skip-turbolinks --skip-action-mailer --skip-action-mailbox --skip-active-storage --skip-test -d mysql
yarn add vuex
bin/rails g controller pages index
vi app/javascript/app.vue
vi app/javascript/child.vue
vi app/javascript/sotre.js
vi app/javascript/packs/hello_vue

vi app/views/layouts/application.html.erb
vi app/views/pages/index.html.erb
vi config/database.yml
vi config/routes.rb
curl http://localhost:3000/
```


```app/javascript/app.vue
<template>
  <div>
    <p>{{ msg }} <span>{{ count }}</span></p>
    <MyChild msg="CHILD COMPONENT"/>
    <input type="button" value=" + " v-on:click="increment" />
    <input type="button" value=" - " v-on:click="decrement" />
  </div>
</template>
<script>
import MyChild from 'child.vue'
export default {
  data: function () {
    return {
      msg: "PARENT COMPONENT"
    }
  },
  
  components: {
    MyChild
  },
  
  computed: {
    count() {
      return this.$store.getters['getCount'];
    }
  },
  
  methods: {
    increment() {
      this.$store.dispatch('countAction', 1)
    },
    
    decrement() {
      this.$store.dispatch('countAction', -1);
    }
  }
}
</script>
<style scoped>
</style>
```


```app/javascript/child.vue
<template>
  <p>{{ msg }} <span>{{ count }}</span></p>
<template>
<script>
export default {
  props: ['msg'],
  computed: {
    count(){
      return this.$store.getters['getCount'];
    }
  }
}
</script>
<style scoped>
</style>
```


```app/javascript/store.js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({

  state: {
    count: 0
  },
  
  mutations: {
    setCount(state, payload) {
      state.count = state.count + payload;
    }
  },
  
  getters: {
    getCount(state) {
      return state.count;
    }
  },
  
  actions: {
    countAction(context, payload) {
      context.commit('setCount', payload)
    }
  }
})
```


```app/javascript/packs/hello_vue.js
import Vue from 'vue'
import App from '../app.vue'
import store from '../store'

document.addEventListener('DOMContentLoaded', () => {

  const app = new Vue({
    store: store,
    render: h => h(App)
  }).$mount()
  
  document.getElementById('root').appendChild(app.$el)
})
```

```app/views/layouts/application.html.erb
<!DOCTYPE html>
<html>
  <head>
    <title>Myapp</title>
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>
    <%= stylesheet_link_tag 'application', media: 'all' %>
    <%= javascript_pack_tag 'application' %>
    <%= javascript_pack_tag 'hello_vue' %>
  </head>
  
  <body>
    <%= yield %>
  </body>
</html>
```

```app/views/pages/index.html.erb
<div id="root"></div>
```

```config/routes.rb
Rails.application.routes.draw do
  get 'pages/index'
  root to: 'pages#index'
end
```


```config/database.yml
```

```sh
curl https://localhost:3000/
```

######

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

```

