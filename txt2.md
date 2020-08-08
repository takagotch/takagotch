######
---



```sh
vi Gemfile
# gem 'postgresql'
bundle install

su -
yum install -y postgreql

postgres --version
```

```sh
which postgres
vi ./bash_profile
export PGDATA=/usr/local/var/progres
source ~/.bash_profile

createuser tky
createuser tky -p
create tky_development -O tky
create tky_test -O tky
// production heroku db postgresql


psql -c 'select * from pg_user' postgres




```

```
yum list | grep postgresql-server
sudo yum install -y https://yum.postgresql.org/9.6/redhat/rhel-7-x86_64/pgdg-pgdg-redhat96-9.6-3.noarch.rpm
sudo yum install -y postgresql96-server postgresql196-contrib

psql --version
sudo /usr/pgsql-9.6/bin/postgresql96-setup initdb

systemctl start postgresql-9.6.service
systemctl enable postgresql-9.6.service
```


