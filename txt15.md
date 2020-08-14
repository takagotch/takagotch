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


```
```

```
```

