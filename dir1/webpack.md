
###### webpack
```
yarn add vue --save
yarn add webpack webpack-cli -D
yarn add @babel/core @babel/polyfill @babel/preset-env babel-loader -D
yarn add css-loader file-loader mini-css-extract-plugin pug pug-plain-loader sass-loader vue-loader vue-style-loader vue-template-compiler webpack-manifest-plugin -D

yarn run build:dev

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
  end
  
  def manifest
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


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```

