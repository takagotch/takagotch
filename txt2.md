###### postgresql
---

```sh
bin/rails db:system:cahnge
rails db:system:change --to=mysql
rails db:system:change --to=sqlite3
bin/rails db:system:change --to=postgresql

// vi Gemfile
// cat config/database.yml
```


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



sudo yum inxgzll -y postgresql-server
psql --version
sudo service postgresql initdb
service postgresql start
sudo passwd postgres
su - 
postgres
createuser -P tky
psql
\du
\q
psql
create database devdb;
\l
\q
psql
\pawwword
\q
sudo vi /var/lib/pgsql/data/pg_hba.conf
sudo service postgresql stop
sudo service postgresql start
sudo yum install -y postgresql-devel
vi Gemfile
# gem 'pg'
bundle install
vi database.yml
+ development:
+   adapter: postgresql
+   encoding: utf8
+   pool: 5
+   username: tky
+   password: password
+   database: devdb
cd ~/apptky
rails g scaffold User name:string age:integer
rake db:migrate
rails s
https://localhost:3000/users
sudo systemctl enable postgresql
sudo systemctl status postgresql
```

```sh
yum -y install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
ls -l /usr/pgsq-12
systemctl enable postgresql-12.service
vi /usr/lib/systemd/system/postgresql-12.service
systemctl daemon-reload
PGSETUP_INITDB_OPTIONS="-E UTF8 --no-locale" /usr/pgsql-12/bin/postgresql-12-setup initdb
cat /data/PG_VERSION
su - postgres
vi /var/lib/pgsql/.pgsql_profile
vi /var/lib/pgsql/.bash_profile
source ~/.bash_profile
pg_ctl start // systemctl start postgresql-12
psql -v
psql -l
createuser --login --pwprompt testuser
createdb --owner=testuser testdb
vi /data/postgresql.conf
vi /data/pg_hba.conf
pg_ctl reload
psql
psql testdb testuser
create table test (id int, value text);
insert into test (id, value) values (1, 'test text');
select * from test;
\d
\du
vi /data/postgresql.conf
vi /data/postgresql.conf
pgbench -i -s 10 testdb
pgbench -c 10 -j 10 -t 2000 -N testdb
yum --showuplicates search postgresql12-server
yum -y install postgresql12-devel
yum -y install epel-release centos-release-scl




yum list | grep postgresql-server
sudo yum install -y https://yum.postgresql.org/12.3/redhat/rhel-7-x86_64/pgdg-pgdg-redhat96-12.3.noarch.rpm
sudo yum install -y postgresql96-server postgresql196-contrib

psql --version
sudo /usr/pgsql-12.3/bin/postgresql96-setup initdb

systemctl start postgresql-12.3.service
systemctl enable postgresql-12.3.service

psql -l

sudo passwd postgres
su - postgres
psql
create role {tky} login createdb password '{password}';
\q
exit
psql -l

systemctl stop postgresql-12.3.service

```

```yml
// database.yml
development:
  adapter: sqlite3
  database: db/apptky_development.sqlite3
  pool: 5
  timeout: 5000
  
test:
  adapter: sqlite3
  database: db/apptky_test.sqlite3
  pool: 5
  timeout: 5000
  
production:
  adapter: postgresql
  encoding: unicode
  pool: 5
  database: apptky_production
  username: <%= ENV['APPTKY_DATABASE_URLNAME'] %>
  password: <%= ENV['APPTKY_DATABASE_PASSWORD'] %>
  # url: <%= ENV['DATABASE_URL'] %>
```

---
###### webpacker

```sh
bin/webpack(-dev-server)
bundle exec rake webpacker:compile
bundle exec rake assets:precompile
```


```rb
# webpacker/lib/webpacker/webpack_runner.rb
require "shellwords"
require "webpacker/runner"

module Webpacker
  class WebpackerRunner < Webpacker::Runner
    def run
       env = { "NODE_PATH" => @node_modules_path.shellescape }
       cmd = [ "#{@node_modules_path}/.bin/webpack", "--config", @webpack_config ] + @argv
       
       Dir.chdir(@app_path) do
         exec env, *cmd
       end
    end
  end
end
 
./node_modules/.bin/webpack --config/webpack/development.js
vi









vi app/views/layouts/application.html.erb
+ <%= javascript_pack_tag "vendor" %>
```

