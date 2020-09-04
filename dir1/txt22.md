###### redis-store
---



```sh
sudo yum install -y redis
redis-server
redis-cli

SET mykey "KEY"
GET mykey
quit

rails new redis_cache
vi Gemfile
+ gem 'redis-rails'
bundle install

```

```config/application.rb
config.cache_store = :redis_store, 'redis://localhost:6379/0/cache', { expiration_in: 90.minutes }
```

```config/initializers/redis.rb
Redis.current = Redis.new
Redis.new(:host => '127.0.0.1', :port => 6379)
```

```app/controllers/redis_controller.rb
class RedisController < ApplicationController
  def show
    Redis.current.set('mykey', 'key')
  end
end


```

```app/views/redis/show.html.erb
<%= Redis.current.get('mykey') %>
```

```
```

```
```

```config/initializers/session_store.rb
RailsGameServer::Application.config.session_store :redis_store, :servers => { :host => "localhost", :port => 6379, :namespace => "session" }
```

```sh
redis-cli
keys sessions*
get sessions:xxxxx
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
```

```
```

```
```

```
```

```
```

