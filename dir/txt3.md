#### sidekiq, resque
---


```app/workers/event_worker.rb
class EventWorker
  include Sidekiq::Worker
  sidekiq_options queue: :event
  
  def perform()
    puts "perform"
  end
end

```

```sh
rails g job generate_ticket

bundle exec sidekiq

sudo apt-get -y update && sudo apt-get install -y redis-server
sudo systemctl start redis-server
sudo systemctl status redis-server
sudo systemctl enable redis-server
cat app/Gemfile
+ gem 'sidekiq'
bundle install
bundle exec sidekiq

rails c --sandbox
```

```app/jobs/application_job.rb
class ApplicationJob < ActiveJob::Base
end
```

```app/jobs/generate_ticket_job.rb
class GenerateTicketJob < ApplicationJob
  queue_as :default
  
  def perform(*args)
    p "Hello Active Job."
  end
end

```

```.rb
GenerateTicketJob.perform_later
GenerateTicketJob.perform_later some_arg
GenerateTicketJob.set(wait: 5.second).perform_later

GenerateTicketJob.new.perform
```

```.sh
Sidekiq::Stats.new.to_json
p Sidekiq::Stats.new.to_json

Sidekiq::RetrySet.new.each {|job| puts "#{job.jid} #{job.klass} #{job.args}"}
Sidekiq::RetrySet.new.find_job('xxx')
# => #<Sidekiq::SortedEntry:xxxx @args=nil,
        @value="",
        \"">
Sidekiq::RetrySet.new.each {|job| p job}

Sidekiq::RetrySet.new.find_job(<JID>).delete
Sidekiq::RetrySet.new.find_job('xxxx').delete
Sidekiq::RetrySet.new.clear
Sidekiq::RetrySet.new.clear

curl http://localhost:3000/sidekiq/
```

```.rb
require 'sidekiq/api'

Sidekiq::Queue, Sidekiq::RetrySet
```


```config/application.rb
 : <snip>
module Rails52SampleApp
  class Application < Rails::Application
    : <snip>
   #
   config.active_job.queue_adapter = :sidekiq
  end
end

```

```app/jobs/generate_ticket_job.rb
class GenerateTicketJob < ApplicationJob
  queue_as :default
  
  def perform
    ticket = Ticket.create!(name: 'xxx-xxx-xxx-xxx-xxx')
    p "Generating ticket #{ticket.name} ..."
  end
end

class TrashableCleanupJob < ApplicationJob
  def perform(trashable, depth)
    trashable.cleanup(depth)
  end
end

```

```routes.rb
require 'sidekiq/web'
mount Sidekiq::Web => '/sidekiq'
```

```
```

```
```

```.sh
./bin/rails g job guests_cleanup

CreateTicketsJob.perform_later("CreateTickets")
CreateTicketsJob.set(wait: 1.week).perform_later(record)

sudo yum install -y redis
sudo service redis start
vi Gemfile
+ gem 'sidekiq'
+ gem 'sinatra', require: false
+ gem 'redis-namespace'
bundle install

rails g sidekiq:worker EventWorker
bundle exec sidekiq

rails c
CreateTicket.perform_async('Create_ticket')
CreateTicket.perform_in(3.hours, 'CreateTicket', 1)
CreateTicket.perform_at(3.hours.from_now, 'CreateTicket', 1)

bundle exec cap production deploy
bundle exec cap production sidekiq:start

sudo yum install -y monit
sudo chkconfig monit on

sudo service monit start

sudo monit status

vi Gemfile
+ gem 'resque', :require => 'resque/server'
bundle install

rails c
Resque.enqueue(CreateTicket, "CreateTicekt")


```

```app/jobs/create_tickets_job.rb
class CreateTicketsJob < ActiveJob::Base
  queue_as :default
  
  def perform(*args)
    p " #=> result "
  end
end

```

```config/initializers/sidekiq.rb
Sidekiq.configure_server do |config|
  case Rails.env
    when 'production' then
      config.redis = { url: 'redis://prg.redis-example.com:6379', namespace: 'sidekiq' }
    when 'staging' then
      config.redis = { url: 'redis://stg.redis-example.com:6379', namespace: 'sidekiq' }
    else 
      config.redis = { url: 'redis://127.0.0.1:6379', namespace: 'sidekiq' }
  end
end

Sidekiq.configure_client do |config|
  case Rails.env
    when 'production' then
      config.redis = { url: 'redis://prd.redis-example.com:6379', namespace: 'sidekiq' }
    when 'staging' then
      config.redis = { url: 'redis://stg.redis-exmple.com:6379', namespace: 'sidekiq' }
    else 
      config.redis = { url: 'redis://127.0.0.1:6379', namespace: 'sidekiq' }
    end
  end
end
```

```app/workers/EventWorker.rb
class EventWorker
  include Sidekiq::Worker
  
  def perform(data)
    Rails.logger.debug(data)
  end
  
end


```

```config/routes.rb
require 'sidekiq/web'
mount Sidekiq::Web => '/sidekiq'
```

```Gemfile
group :development, :test do
  gem 'capistrano-sidekiq', github: 'seuros/capistrano-sidekiq'
end
```

```config/deploy/production.rb
set :rails_env, "production"
set :unicorn_rack_env, "production"
set :pty, false

role :app, %w{vagrnnt@192.168.33.10}
role :web, %w{vagrant@192.168.33.10}
role :db,  %w{vagrant@192.168.33.10}

server '', user: '', roles: %w{web app}

set :ssh_options, {
  keys: %w{/home/vagrant/.ssh/id_rsa},
  forward_agent: false
  auth_methods: %w{publickey}
}
```

```Capfile
require 'capistrano/sidekiq'
```

```config/deploy/production.rb
set :sidekiq_role, :batch
role :batch, %w{vagrant@192.168.33.10}
server '192.168.33.10', user: 'vagrant', roles: %w{batch}
```

```/etc/monit.conf
set httpd port 2812 adn
  allow localhost
```

```.rb
require 'capistrano/sidekiq/monit'
```

```config/initializers/resque.rb
case Rails.env
  when 'production' then
    Resque.redis = 'prd.redis-example.com'
  when 'staging' then
    Resque.redis = 'stg-example.com'
  else
    Resque.redis = '127.0.0.1'
end
Resque.redis.namespace = "resque:prj-name:#{Rails.env}"

```

```app/workers/CreateTicket.rb
class CreateTicket
  @queue = :default
  
  def self.perform(data)
    Rails.logger.debug(data)
  end
end

```

```lib/tasks/resque.rake
require 'resque/tasks'
```

```config/routes.rb
mount Resque::Server, :at => "/resque"
```

```
```

```
```

###### resque .rb
```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```



