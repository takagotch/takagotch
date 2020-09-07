
###### webpack
Delete Rails-webpacker and then try to get down webpack on vue.

```
yarn add vue --save
yarn add webpack webpack-cli -D
yarn add @babel/core @babel/polyfill @babel/preset-env babel-loader -D
yarn add css-loader file-loader mini-css-extract-plugin pug pug-plain-loader sass-loader vue-loader vue-style-loader vue-template-compiler webpack-manifest-plugin -D

yarn run build:dev
yarn run webpack-dev-server --mode development
rails s
vi Gemfile
+ gem "foreman"
bundle install
vi Procfile

bundle exec foreman start -p 3000
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

```app/helpers/webpack_bundle_helper.rb
require "open-uri"

module WebpackBundleHelper
  class BundleNotFound < StandardError; end
  
  def javascript_bundle_tag(entry, **options)
    path = asset_bundle_path("#{entry}.js")
    
    options = {
      src: path,
      defer: true,
    }.merge(options)
    
    options.delete(:defer) if options[:async]
    
    javascript_include_tag "", **options
  end
  
  def stylesheet_bundle_tag(entry, **options)
    path = asset_bundle_path("#{entry}.css")
    
    options = {
      href: path,
    }.merge(options)
    
    stylesheet_link_tag "", **options
  end
  
  private
  
  def asset_server
    port = Rails.env === "development" ? "3035" : "3000"
    "http://#{request.host}:#{port}"
  end
  
  def pro_manifest
    File.read("public/assets/manifest.json")
  end
  
  def dev_manifest
    OpenURI.open_uri("#{asset_server}/public/assets/manifest.json")
  end
  
  def test_manifest
    File.read("public/assets-test/manifest.json")
  end
  
  def manifest
    return @manifest ||= JSON.parse(pro_manifest) if Rails.env.production?
    return @manifest ||= JSON.parse(dev_manifest) if Rails.env.devlopment?
    return @manifest ||= JSON.parse(test_manifest)
  end
  
  def valid_entry?(entry)
    return true if manifest.key?(entry)
    raise BundleNotFound, "Could not find bundle with name #{entry}"
  end
  
  def asset_bundle_path(entry, **options)
    valid_entry?(entry)
    asset_path("#{asset_server}/public/assets/" + manifest.fetch(entry), **options)
  end
end
```


```layout/application.haml
- = stylesheet_link_tag 'application', media: 'all'
- = javascript_include_tag 'application'

+ = javascript_bundle_tag "#{params[:controller]}#{params[:action].capitalize}"
+ = stylesheet_bundle_tag "#{params[:controller]}#{params[:action].capitalize}"
```

```lib/tasks/assets_path_proxy.rb
require "rack/proxy"

class AssetsPathProxy < Rack::Proxy
  def perfom_request(env)
    if env["PATH_INFO"].include?("/images/")
      if Rails.env != "production"
        dev_server = env["HTTP_HOST"].gsub(":3000", ":3035")
        env["HTTP_HOST"] = dev_server
        env["HTTP_X_FORWARDED_HOST"] = dev_server
        env["HTTP_X_FORWARDED_SERVER"] = dev_server
      end
      env["PATH_INFO"] = "/public/assets/images/" + env["PATH_INFO"].split("/").last
      super
    else
      @app.call(env
    end
  end
end
```


```config/environment/development.rb
config.middleware.use AssetsPathProxy, ssl_verify_none: true
```

```
```


```layout/application.haml
!!!
%html
  %head
    %meta{ charset: "utf-8" }
    %meta{ name: "viewport", content: "width=devive-width"}
    %title
      MieMa
    = csrf_meta_tags
    = csp_meta_tag
    = javascript_bundle_tag "#{params[:controller]#{params[:action].capitalize}}"
    = javascript_bundle_tag "#{params[:controller]#{params[:action].capitalize}}"
  %body
    #app  // <div id="app">
      = yield
```

```index.haml
%root-index
```


```components/app.vue
<template lang="pug">
  div
    p {{ message }}
</template>

<script>
export default {
  data() {
    return {
      message: 'Hello Vue!'
    }
  }
}
</script>

<style>
p {
  font-size: 2em;
  text-align: center;
  background-color: greenyellow;
}
</style>
```

```pages/root/index.vue
<template lang="pug">
#root-index
  App
  p Index page
  img(src='../../images/image.jpg')
</template>

<style lang="scss"></style>

<script>
  import App from '../../componets/app'
  
  export default {
    componets: {
      App
    }
  }
</script>
```


```pages/root/rootIndex.js
import Vue from 'vue'
import RootIndex from './index'

new Vue({
  el: '#app',
  components: {
    RootIndex  // <root-index />
  }
})
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```

