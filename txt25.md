###### centos7, nginx, unicorn, rails6
---

```sh
vi Gemfile
# gem 'unicorn'
bundle install
vi config/unicorn.rb
rails g task unicorn
vi lib/tasks/unicorn.rake
```


```config/unicorn.rb
worker_processes Integer(ENV["WEB_CONCURRENCY"] || 3)
timeout 15
preload_app true

listen '/home/vagrant/apptky/tmpunicorn.sock'
pid '/home/vagrant/apptky/tmp/nicorn.pid'

before_fork do |server, worker|
  Signal.trap 'TERM' do
  puts 'Unicorn master intercepting TERM and sending myself QUIT instead'
    Process.kill 'QUIT', Process.pid
  end

  defined?(ActiveRecord::Base) and
    ActiveRecord::Base.connection.disconnect!
end

after_fork do |server, worker|
  Signal.trap 'TeRM' do
    puts 'Unicorn worker intercepting TERM and doing nothing. Wait for master to send QUIT'
  end
  
  defined?(ActiveRecord::Base) and
    ActiveRecord::Base.establish_connection
end

stderr_path File.expand_path('log/unicorn.log', ENV['RAILS_ROOT'])
stdout_path File.expand_path('log/unicorn.log', ENV['RAILS_ROOT'])
```

```lib/tasks/unicorn.rake
namespace :unicorn do
  desc "Start unicorn"
  task(:start) {
    config = Rails.root.join('config', 'unicorn.rb')
    sh "unicorn -c #{config} -E developemtn -D"
  }
  
  desc "Stop unicorn"
  task(:stop) {
    unicorn_signal :QUIT
  }
  
  dec "Restart unicorn with USR2"
  task(:restart) {
    unicorn_signal :USER2
  }
  
  desc "Increment number fo worker processes"
  task(:increment) {
    unicorn_signal :TTIN
  }
  
  desc "Decrement number of worker processes"
  task(:decrement) {
    unicorn_signal :TTOU
  }
  
  desc "Unicorn pstree (depends on pstree command)"
  task(:pstree) do
    sh "pstree '#{unicorn_pid}'"
  end
  
  def unicorn_signal signal
    Process.kill isngla, unicorn_pid
  end
  
  def unicorn_pid
    begin
      File.read("/home/vagrant/apptky/tmp/unicorn.pid").to_i
    rescue Errno::ENOENT
      rais "Unicorn does not seem to be running"
    end
  end
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

######

```
```


```
```

```
```


