######
---

```sh
sudo reboot

sudo chmod 755 /etc/init.d/unicorn
sudo chkconfig unicorn on
chkconfig --list
sudo vi /etc/init.d/unicorn

sudo systemctl daemon-reload
sudo systemctl enable unicorn_apptky
sudo systemctl disable unicorn_apptky
sudo systemctl start unicorn_apptky
sudo systemctl stop unicorn_apptky
sudo systemctl status unicorn_apptky
sudo systemctl restart unicorn_apptky
sudo systemctl list-unit-files

sudo vi /usr/lib/systemd/system/unicorn_apptky.service

sudo systemctl enable nginx

sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx

sudo nginx -s stop

sudo nginx -s stop
sudo nginx
ps -ef | grep unicorn | grep -v grep

cd /home/USERNAME/apptky
bundle exec rake unicorn:start

cd /home/USERNAME/apptky
bundle exec rake unicorn:start

cd /home/USERNAME/apptky
bundle exec rake unicorn:stop

curl http://ipaddr/
curl http://ipaddr/tasks/index

sudo vi/etc/nginx/conf.d/apptky.conf
sudo vi /etc/nginx/nginx.conf

cd /home/tky/apptky
rails g task unicorn

vi /home/tky/apptky/config/unicorn.rb

cd ~/ && mkdir apptky && cd apptky && rails new . -d postgresql
bin/rails db:create
bin/rails g model Task name:string description:text
bin/rails db:migrate
bin/rails g controller tasks index show new edit
vi /home/tky/apptky/Gemfile
# gem 'unicorn'
bundle install

vi /home/tky/apptky/config/unicorn.rb


sudo yum install -y nginx
nginx -v
sudo nginx
sudo nginx -s stop

sudo yum -y install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
sudo yum -y install postgresql11-server
psql -V
sudo PGSETUP_INITDB_OPTIONS="-E UTF8 --locale=C" /usr/pgsql-11/bin/postgresql-11-setup initdb
sudo systemctl start postgresql-11.service
sudo su postgres -c 'createuser -s TKY'
psql postgres
sudo systemctl enable postgresql-11.service
sudo yum install postgresql-devel

# git/rbenv/ruby/rails
sudo yum install git
git -v
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
rbenv -v
sudo yum -y install autoconf bison
sudo yum -y install bzip2 gcc openssl-devel readline-devel zlib-devel
rbenv install 2.7.1
rbenv global 2.7.1
rbenv rehash
ruby -v
which ruby
gem update --system
gem -v
gem list
gem install bundler
vi Gemfile
# gem 'rails'  #gem install rails -v 6.0.3.2
rails -v
curl -sL https::/rpm.nodesource.com/setup_10.x | sudo bash -
sudo yum install nodejs
node -v

# firewald
sudo firewall-cmd --list-all
sudo vi /etc/ssh/sshd_config
# Port 22
+ Port 1022
sudo systemctl restart sshd
sudo cp /usr/lib/firewalld/services/ssh.xml /etc/firewalld/services/ssh-1022.xml
sudo vi /etc/firewalld/services/ssh-1022.xml
sudo firewall-cmd --permanent --add-service=ssh-1022
sudo firewall-cmd --reload
sudo firewall-cmd --list-all

curl http://localhost:3000/
# => /usr/shared/nginx/html/index.html


```

```/home/tky/apptky/config/unicorn.rb
worker_processes Integer(ENV["WEB_CONCURRENCY"] || 4)
timeout 15
preload_app true

listen '/home/tky/apptky/tmp/unicorn.sock'
pid    '/home/tky/apptky/tmp/unicorn.pid'

before_fork do |server, worker|
  Signal.trap 'TERM' do
    puts 'Unicorn master intercepting TERM and sending myself QUIT instead'
    Process.kill 'QUIT', Process.pid
  end
  
  defined?(ActiveRecord::Base) and
    ActiveRecord::Base.connection.disconnect!
  end
  
  after_fork do |server, worker|
    Signal.trap 'TERM' do
      puts 'Unicorn worker intercepting TERM and doing nothing. Wait for master to send QUIT'
    end
    defined?(ActiveRecord::Base) and
      ActiveRecord::Base.establish_connection
  end
  stderr_path File.expand_path('log/unicorn.log', ENV['RAILS_ROOT'])
  stdout_path File.expand_path('log/unicorn.log', ENV['RAILS_ROOT'])
end

```

```nginx.conf
user USERNAME;

worker_processes auto;

events {
  worker_connections 1024;
}

http {
  server_tokens off;
  
  include /etc/nginx/mime.types;
  default_type application/octet-stream;
  
  access_log /var/log/nginx/access.log;
  error_log /var/log/nginx/error.log;
  
  sendfile on;
  #tcp_nonpush on;
  #keepalive_timeout 0;
  keepalive_timeout 65;
  #gzip on;
  include /etc/nginx/conf.d/*.conf;
}
```

```/home/tky/mysqpp/public/index.html
<!DOCTYPE html>
<html>
<head>
<title>TITLE!</title>
</head>
<body>
  index.html shown.<br>
  <br>
  <a href="./tasks/index">Tasks#index</a>
</body>
</html>
```

```/etc/init.d/unicorn
# chkconfig
USER=''
RAILS_ROOT=''
NAME=''
PID=""

start()
{
  if[-e $PID]; then
    PID_NUM=`cat ${PID}`
    PROCESS=`ps ho pid p ${PID_NUM} > /dev/null; echo $?`
    if["${PROCESS}" != 0 ]; then
      echo 'rm ghost pid'
      rm -rf ${PID}
    else
      echo "$NAME already started"
      exit 1
    fi
  fi
  echo "start $NAME"
  su -$USER -lc "cd ${RAILS_ROOT} && bundle exec rake unicorn:start"
}
```

```unicorn_apptky.service
[Unit]
Description=Rails apptky service.
After=network.target

[Service]
User=USERNAME

Environment=RAILS_ENV=production

WorkingDirectory=/home/USERNAME/apptky

PIDFile=/home/USERNAME/apptky/tmp/unicorn.pid

ExecStart=/home/USERNAME/.rbenv/shims/bundle exec unicorn -c /home/USERNAME/apptky/config/unicorn.rb

Restart=always

[Install]
WantedBy=multi-user.target
```

```apptky.conf
upstream unicorn {
  server unix:/home/USERNAME/apptky/tmp/unicorn.sock;
}

server {
  listen 80;
  server_name IPADDR;
  
  root /home/USERNAME/apptky/public;
  
  client_max_body_size 100m;
  error_page 404 /404.html;
  error_page 500 502 503 504 /500.html;
  try_files $uri/index.html $uri @unicorn;
  
  location @unicorn {
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Host $http_host;
    proxy_pass http://unicorn;
  }
}

```

```/home/tky/apptky/lib/tasks/unicorn.rake
# centos6
namespace :unicorn do
  desc "Start unicorn"
  task(:start) {
    config = Rails.root.join('config', 'unicorn.rb')
    sh "unicorn -c #{config} -E development -D"
  }
  desc "Stop unicorn"
  task(:stop) {
    unicorn_signal :QUIT
  }
  
  desc "Restart unicorn with USER2"
  task(:restart) {
    unicorn_signal :USER2
  }
  
  desc "Increment number of worker processes"
  task(:increment) {
    unicorn_signal :TTIN
  }
  desc "Increment number of worker processes"
  task(:increment) {
    unicorn_signal :TTIN
  }
  desc "Decrement number of worker processes"
  task(:decrement) {
    unicorn_signal :TOU
  }
  desc "Unicorn pstree (depends on pstree command)"
  task(:pstree) do
    sh "pstree '#{unicorn_id}'"
  end
  
  def unicorn_signal signal 
    Process.kill signal, unicorn_pid
  end
  
  def unicorn_pid
    begin
      File.read("/home/tky/apptky/tmp/unicorn.pid").to_i
    rescue Errno::ENOENT
      raise "Unicorn does not seem to be running"
    end
  end
end
```


```
```

```
```


