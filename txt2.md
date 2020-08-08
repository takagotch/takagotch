###### postgresql
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


