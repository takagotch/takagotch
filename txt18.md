###### ActiveStorage
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
```app/sessions/confirm_ip.rb
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

###### ActiveStorage multi
```app/models/upload.rb
class Upload < ApplicationRecord
  has_many_attached :attachments
end

```

```.sh
rails new . --skip-turbolinks --skip-action-mailer --skip-action-mailbox --skip-test -d mysql
vi config/database.yml
bin/rails active_storage:install
bin/rails g scaffold Upload name:string description:text
bin/rails db:migrate

curl http://localhost:3000/uploads
```

```app/views/uploads/_form.html.erb
<div class="field">
  <%= form.label "tempfile possible to multi uploads"%>
  <%= form.file_field :attachments, multiple: true %>
</div>
```

```app/views/uploads/show.html.erb
<p>
  <strong>UPLOAD FILE:</strong>
  <% if @cat.attachments.attached? %>
    <% @cat.attachments.each do |obj| %>
    <% url = rails_blob_path(obj) %>
    <br>
    <%= link_to URI.unescape(File.basename(url)),url %>
    <% end %>
  <% end %>
</p>
```


```app/controllers/uploads/upload_controller.rb
def create
  @upload = Upload.new(upload_params)
  @upload.attachments = params[:upload][:attachments]
  if @upload.save
    redirect_to @upload, notice: 'REGISTRAION!'
  else
    render :new
  end
end
def update
  @upload = Upload.where(id: params[:id])
  @upload[0].name = params[:upload][:name]
  @upload[0].description = params[:upload][:description]
  @upload[0].attachments = params[:upload][:attachments]
  @upload = @upload[0]
  
  if(!@upload.valid?)
    render :edit
    return
  end
  if @upload.save
    redirect_to @upload, notice: 'UPDATED!'
  else
    render :edit
  end
end

```

###### ActiveStorage single
```sh
rails new . --skip-turbolinks --skip-action-mailer --skip-action-mailbox --skip-test -d mysql
vi config/database.yml
bin/rails active_storage:install
bin/rails g scaffold Upload name:string description:text
bin/rails db:migrate

curl http://localhost:3000/
```

```app/models/upload.rb
class Upload < ApplicationRecord
  has_one_attached :attachment
end
```

```app/views/uploads/_form.html.erb
<div>
  <%= form.label "TEMPTED FILE" %>
  <%= form.file_field :attachment %>
</div>
```

```app/views/uploads/show.html.erb
<p>
  <strong>IMG:</strong>
  <% if @upload.attachment.attached? && @Attachment_image %>
    <%= image_tag @upload.attachment %>
  <% end %>
</p>
<p>
  <strong></strong>
  <% if @cat.attachment.attached? %>
    <%= link_to "TEMPTED FILE(" + @ext +")", rails_blob_path(@upload.attachment) %>
  <% end %>
</p>
```

```app/controllers/uploads/uploads_controller.rb
def create
  @upload = Upload.new(cat_params)
  @upload.attachment = params[:cat][:attachment]
  
  if @upload.save
    redirect_to @cat, notice: 'REGISTRATION!'
  else
    render :new
  end
end

def update
  @upload = Upload.where(id: params[:id])
  @upload[0].name = params[:upload][:name]
  @upload[0].description = params[:upload][:description]
  @upload[0].attachment = params[:upload][:attachment]
  @upload = @upload[0]

  if (!@upload.valid?)
    render :edit
    return
  end
  if @upload.save
    redirect_to @upload, notice: 'UPDATED!'
  else
    render :edit
  end
end

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


###### ActiveStorage
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
