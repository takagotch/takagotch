###### [config](https://github.com/rubyconfig/config)
---


```config/settings.yml
service:
  name: 'tkgcci'
  url: 'http://tkgcci.com'

authentication_password: "xxx"
```

```.sh
rails c

rails g config:install


```

```config/application.rb
module Apptky
  class Application < Rails::Application
    config.load_defaults 5.1
  end
end

```


```app/views/users/_form.html.erb
<%= form_with() do |form| %>
  <%= form.text_field :email %>
<% end %>
```

```#=> output
<form action="/users" ...>
  <input type="text" name="user[email]">
  <input id="user_mail" type="text" name="user[email]">
</form>
```

```config/initializers/new_framework_defaults_5_2.rb
Rails.application.config.action_view.form_with_generates_ids = true # false
```


```config/initializers/config.rb
```

```
```

```
```


```#{Rails.root}/config/environments/development.yml
size: 2
computed: <%= 1 + 2 + 3 %>
section:
  size: 3
  servers: [ {name: tkgcci.com}, {name: amazon.com} ]
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

