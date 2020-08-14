###### Vue Router
---

```
cd ~/ && mkdir apptky && cd apptky && rails new . --webpack=vue --skip-turbolinks --skip-turbolinks --skip-action-mailer --skip-action-mailbox --skip-active-storage --skip-test -d my

yarn add vue-router
bin/rails g controller pages index

curl http://localhost:3000/
```

```
vi app/javascript/app.vue
vi app/javascript/router.js
vi app/javascript/components/article.vue
vi app/javascript/components/home.vue
vi app/javascript/packs/hello_vue.js
```


```config/database.yml
```

```config/routes.rb
Rails.application.routes.draw do
  get 'pages/index'
  root to: 'pages#index'
  
  get 'article/:id',to:'pages#index'
end


```


```app/views/pages/index.html.erb
<div id="root"></div>
```

```app/views/layouts/application.html.erb
<!DOCTYPE html>
<html>
<head>
  <%= csrf_meta_tags %>
  <%= csp_meta_tag %>
  <%= stylesheet_link_tag 'application',media:'all' %>
  <%= javascript_pack_tag'application' %>
  <%= javascript_pack_tag 'hello_vue' %>
</head>
<body>
  <%= yield %>
<body>
</html>
```

