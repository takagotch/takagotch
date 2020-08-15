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

# firewald

curl http://localhost:3000/
# => /usr/shared/nginx/html/index.html


```

```/home/tky/apptky/config/unicorn.rb


```

```/home/tky/myapp/config/unicor

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


```


```
```

```
```


