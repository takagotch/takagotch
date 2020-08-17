###### aws err rails6
---

```sh
git make gcc-c++ patch
sudo yum install git make gcc-c++ patch openssl-devel libyaml-devel libffi-devel libicu-devel libxml2 libxslt libxml2-devel libxslt-devel zlib-devel readline-devel mysql mysql-server mysql-devel ImageMagick ImageMagick-devel epel-release

sudo mkdir www

cat aws_git_rsa.pub

ssh-keygen -t rsa

sudo service mysqld start
sudo yum install -y mariadb-server
sudo systemctl enable mariadb
sudo service mariadb start

sudo service nginx start
systemctl status nginx.service
sudo nginx -t
sudo service nginx status -l
sudo systemctl status httpd.service
tail -f /var/xyz/rails/tkyapp/log/unicorn.log
tail -f /var/xyz/rails/tkyapp/log/nginx.error.log

ps aux | grep unicorn
kill -9 pid
unicorn_rails -c /var/www/rails/apptky/config/unicorn.conf.rb -D -E production
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


