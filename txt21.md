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

######

```
```


```
```

```
```


