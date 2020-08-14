######
---



```sh
cd ~/ && mkdir apptky && cd apptky && rails new . --webpack=vue --skip-turbolinks --skip-turbolinks --skip-action-mailer --skip-action-mailbox --skip-active-strage --skip-test -d mysql

yarn add babel-polyfill
yarn remove @rails/actioncable
yarn remove @rails/ujs
vi app/javascript/packs/application.js
# require("@rails/ujs").start()
# require("channels")

bin/rails g controller pages index

curl http://localhost:3000/
bin/rails assets:precompile

```

```
headers: {
  'Content-Type': 'application/json',
  'X-CSRF-TOKEN': document.querySelector('meta[name="csrf-token"]').getAttribute('content')
}
```


```config/routes.rb
Rails.application.routes.draw do
  get 'pages/index'
  root to: 'pages#index'
end
```

```app/views/pages/index.html.erb
<div id="root"></div>
```

```app/views/layouts/application.html.erb
<!DOCTYPE html>
<html>
<head>
  <title></title>
  <%= csrf_meta_tags %>
  <%= csp_meta_tag %>
  <%= stylesheet_link_tag 'application', media: 'all' %>
  <%= javascript_pack_tag 'application' %>
  <%= javascript_pack_tag 'hello_vue' %>
</head>
<body>
  <%= yeild %>
</body>
</html>
```

```app/javascript/packs/hello_vue.js
<template>
  <div id="app">
    <p>{{ message }}</p>
  </div>
</template>
<script>
export default {
  data: function () {
    return {
      message: "Hello Vue!"
    }
  }
}
</script>
<style>
p {
  font-size: 2em;
  text-align: center;
}
</style>
```


```app/javascript/app.vue
import "babel-polyfill"
import Vue from 'vue'
import App from '../app.vue'
document.addEventListener('DOMContentLoaded', () => {
  const app = new Vue({
    render: h => h(App)
  }).$mount()
  document.getElementById('root').appendChild(app.$el)
})

```

###### IE11 vue.js rails6
```
yarn add babel-polyfill

```


```.vue.js
import "bable-polyfill";
import Vue from 'vue'
import App from '../app.vue'

import "fetch-polyfill"
```

###### ipaddr rails6
```.rb
require 'resolv'
def isJapan?(ip)
  if (ip == "::1" || ip == "127.0.0.1")
    return true
  end
  host = Resolv.getname(ip)
  if host.split('.')[-1].downcase == "jp"
    true
  else
    false
  end
rescue Resolv::ResolvError
  false
end
```


```
```

```
```


