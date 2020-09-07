###### ActionCable, websocket
---


```channels/chat_message_channel.rb
class ChatMessageChannel < ApplicationCable::Channel
  def subscribed;stream_from "chat_message_channel";end

  def unsubscribed
  end
  
  def speak(data)
    @message = Message.create(body: data["message"])
    data['id'] = @message.id
    ActionCable.server.broadcast 'chat_message_channel',message: data
  end
end


```

```message.js
App.chatmessage = App.cable.subscriptions.create("ChatMessageChannel", {
  connected: function() {
  },
  
  disconnected: function() {
  },
  
  received: function(data) {
    let message_id = data['message']['id']
    let message_text = data['message']['message']
    let html =`
      <tr>
      <td><a href="/messages/${message_id}">Show</td>
      <td><a href="/messages/${message_id}/edit">Edit</td>
      <td><a data-confirm="Are your sure?" rel="nofollow" data-method="delete" href="/messages/${message_id}">Destroy</td>
      <td>${message_text}</td>
      </tr>`
        $("#message").append(html)
  },
  
  speak: function(message) {
    return this.perform('speak',{message: message});
  }
  }, $(document).on('keypress', '[data-behavior~=speak_chat_messages]', function(event) {
  if (event.keyCode === 13) {
    App.chatmessage.speak(event.target.value)
      event.target.value = ''
      event.preventDefault()
  }  
}));
```

```.sh
rails g scaffold 
vi app/views/index.html.erb
```

```app/views/index.html.erb
<p id="notice"><%= notice %></p>

<h1></h1>

<table>
  <thead>
    <tr>
      <th colspan="3"></th>
    </tr>
  </thead>
  
  <tbody id="message">
    <% @message.each do |message| %>
      <tr>
        <td><%= link_to 'Show', message %></td>
        <td><%= link_to 'Edit', edit_message_path(message) %></td>
        <td><%= link_to 'Destroy', message, method: :delete, data: {confirm: 'Are you sure?' } %></td>
        <td><%= message.body %></td>
      </tr>
    <% end %>
  </tbody>
</table>

<br>
<%= link_to 'New Message', new_message_path %>

</div>
<br/>
<form>
  <label>
    websockets SEND: <input type="text" data-behavior="speak_chat_messages">
  </label>
</form>
```


```javascripts/cable.js
(function() {
  this.App || (this.App = {});
  
  App.cable = ActionCable.createConsumer();
  
}).call(this);
```

```message.js
```


```chat_message_channel.rb
```

```
```

###### Action Cable, websocket
```
```

```
```

###### webpack
```
yarn add vue --save
yarn add webpack webpack-cli -D
yarn add @babel/core @babel/polyfill @babel/preset-env babel-loader -D
yarn add css-loader file-loader mini-css-extract-plugin pug pug-plain-loader sass-loader vue-loader vue-style-loader vue-template-compiler webpack-manifest-plugin -D
```

```package.json
"scripts": {
  "build:dev": "webpack --progress --mode=development",
  "build:pro": "webpack --progress --mode=production"
}
```


```webpack.config.js
const path = require('path')
const path = require('glob')
const webpack = require('webpack')
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
const VueLoaderPlugin = require('vue-loader/lib/plugin')
const ManifestPlugin = require('webpack-manifest-plugin')

let entries = {}
glob.sync('./frontend/pages/**/*.js').map(function(file) {
  let name = file.split('/')[4].split('.')[0]
  entries[name] = file
})

module.exports = (env, argv) => {
  const IS_DEV = argv.mode === 'development'
  
  return {
    entry: entries,
    
    output: {
      filename: 'javascripts/[name]-[hash].js',
      path: path.resolve(__dirname, 'public/assets')
    },
    plugins: [
      new VueLoaderPlugin(),
      new MiniCssExtractPlugin({
      
      }).
      plugins: [
        new VueLoaderPlugin(),
        new MiniCssExtractPlugin({
          filename: 'stylesheets/[name]-[hash].css'
        }),
        new webpack.HotModuleReplacementPlugin(),
        new ManifestPlugin({
          writeToFileEmit: true
        })
      ],
      module: {
        rules: [
          {
            test: /\.js$/,
            exclude: /node_modules/,
            loader: 'babel-loader',
            options: {
              presets: [
                [
                  '@babel/preset-env',
                  {
                    targets: {
                      ie: 11
                    },
                    useBuiltIns: 'usage'
                  }
                ]
              ]
            }
          }.
          {
            test: /\.vue$/,
            loader: 'vue-loader'
          },
          {
            test: /.\pug/,
            loader: 'pug-plain-loader'
          },
          {
            test: /\.(c|sc)ss$/,
            use: [
              {
                loader: MiniCssExtractPlugin.loader,
                options: {
                  publicPath: path.resolve(__dirname, 'public/assets/stylesheets')
                }
              },
              'css-loader',
              'sass-loader'
            ]
          },
          {
            test: /\.(jpg|png|gif)/,
            loader: 'file-loader',
            options: {
              name: '[name]-[hash]-[ext]',
              outputPath: 'images',
              publicPath: function(path) {
                return '/images/' + path
              }
            }
          }
        ]
      }
    ],
    resolve: {
      alias: {
        vue: 'vue/dist/vue.js'
      },
      extensions: ['.js', '.scss', 'css', '.vue', '.jpg', '.png', '.gif', ' ']
    },
    optimization: {
      splitChunks: {
        cacheGroups: {
          vendor: {
            test: /.(c|sa)ss/,
            name: 'style',
            chunks: 'all',
            enforce: true
          }
        }
      }
    }
  }
}
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

