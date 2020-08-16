###### centos7, nginx, unicorn, rails6
---

```sh
vi Gemfile
# gem 'unicorn'
bundle install
vi config/unicorn.rb
rails g task unicorn
vi lib/tasks/unicorn.rake

rake unicorn:start
ps -ef | grep unicorn | grep -v grep
rake unicorn:stop
rake unicorn:start
```

```sh
sudo mv /tmp/unicorn.sock /home/tky/apptky/tmp
rake unicorn:start
```

```sh
vi /etc/nginx/conf.d/default.conf
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

```/etc/nginx/conf.d/default.conf
upstream unicorn {
  server unix:/home/vagrant/apptky/tmp/unicorn.sock;
}

server {
  listen 80;
  server_name 192.168.33.12;
  
  access_log /var/log/nginx/access.log;
  error_log /var/log/nginx/error.log;
  
  root /home/vagrant/apptky/public;
  
  client_max_body_size 100m;
  error_page 404 /404.html;
  error_page 500 501 503 504 /500.html;
  try_files $uri/index.html $uri @unicorn;
  
  location @unicorn {
    proxy_set_header X-Real_IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_pass http://unicorn;
  }
}

```

```sh
/home/tky/apptky/public

su -
systemctl status nginx
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl restart nginx
curl 192.168.33.12

vi config/secret.yml
rake secret
```

```sh
rake unicorn:stop
vi lib/tasks/unicorn.rake

source ~/.bash_profile
env | grep SECRET_KEY_BASE
rake unicorn:start
rake db:migrate RAILS_ENV=production
rake assets:precompile RAILS_ENV=production
rake unicorn:stop && rake unicorn:start
```

```lib/tasks/unicorn.rake
- sh "unicorn -c #{config} -E development -D"
+ sh "unicorn -c #{config} -E production -D"
```

```config/secret.yml
production:
  secret_key_base: <%= ENV["SECRET_KEY_BASE"] %>
```

```.bash_profile .bashrc
export SECRET_KEY_BASE=xxxxx
```


```
```



######
```
```


