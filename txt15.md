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

```app/javascript/packs/hello_vue.js
import Vue from 'vue'
import App from ''
import router from ''

document.addEventListener('DOMContentLoaded', () => {
  const app = new Vue({
    router: router,
    render: h => h(App)
  }).$mount()
  document.getElementById('root').appendChild(app.$el)
})
```

```app/javascript/components/home.vue
<template>
<p>{{ message }}</p>
</template>
<script>
export default {
  name: 'home'
  data: function () {
    return {
      message: "Hello Vue!"
    }
  }
}
</script>
<style scoped>
p {
  font-size: 2em;
}
</style>
```

```app/javascript/components/article.vue
<template>
  <p>ARTICLENUMBER: {{ id }}</p>
</template>
<script>
export default {
  name: 'Article',
  data () {
    return {
      id: 0
    }
  },
  created() {
    this.id = this.$route.params.id
  },
  watch: {
    '$route'(to, from) {
      this.id = to.params.id
    }
  }
}
</script>
<style scoped>
</style>
```

```app/javascript/components/home.vue
<template>
  <p>{{ message }}</p>
</template>
<script>
export defaut {
  name: 'home',
  data: function () {
    return {
      message: "Hello Vue!"
    }
  }
}
</script>
<style scoped>
p {
  font-size: 2em;
}
</style>
```

```app/javascript/router.js
import Vue from 'vue'
import Router from 'vue-router'
import Home from './components/home.vue'

Vue.use(Router)

export default new Router({
  mode: 'history',
  base: process.env.BASE_URL,
  routes: [
    {
      path: '/',
      name: 'Home',
      component: Home
    },
    {
      path: '/article/:id',
      name: 'Article',
      
      component: () => import('./components/article.vue')
    },
  ]
})

```

```app/javascript/app.vue
<template>
  <div>
    <div>
      <router-link to="/" tag="button">HOME</router-link>
      <router-link to="/article/5">ARTICLE NO.5</router-link>
      <router-link to="/article/33">ARTICLE NO.33</router-link>
    </div>
    <router-view/>
  </div>
</template>
<script>
</script>
<style scoped>
a {
  color: blue;
  text-decoration: none;
}

a.router-link-exact-active {
  color: black;
  font-weight: bold;
}
</style>
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



