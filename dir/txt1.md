###### rails-i18n

```config/application.rb
require_relative 'boot'
require 'rails/all'

Bundler.require(*Rails.groups)

module BoardApp
  class Application < Rails::Application
    config.time_zone = 'Osaka'
    config.active_record.default_timezone = :local
    #
    config.i18n.default_locale = :ja
    
    config.i18n.load_path += Dir[Rails.root.join('config', 'locales', '**', '*.{rb,yml}').to_s]
  end
end


```

```sh
vi Gemfile
+ gem 'rails-i18n', '~> 5.1'
+ gem 'rails-i18n', '~> 4.0'

```


```config/locales/views/users/ja.yml
ja:
  users:
    index:
      title: 'USER LIST'
    show:
      title: '%{user_name} USER INFO'
    edit:
      title: '%{user_name} USER INFO EDIT'
      
  activerecord:
    models:
      user: USER
      board: BOARD
      
    attributes:
      user:
        id: ID
        first_name: NAME
        last_name: FIRST NAME
        email: EMAIL
        file: PROFILE PIC
        crypted_password: PASS
  attributes:
    created_at: DATE CREATED
    updated_at: DATE UPDATED
```

```app/views/users/index.html.erb
<%= t '.title' %>

<%= t 'users.index.title' %>

<%= l Time.now %>

<%= l nil %>

<%= l nil, default: '' %>

<li><%= User.model_name.human %></li>
<li><%= User.human_attribute_name(:id) %></li>
<li><%= User.human_attribute_name(:email) %></li>


```


```ja.yml
ja:
  dictionary:
    messages:
      hello_user: 'HELLO%{user_name}'
    words:
      user: &user 'USER INFO'
      user_copy: *user
  helpers:
    user:
      welcome: 'WELCOME!'
  errors:
    nanikore_error: 'ERR!'
  flash:
    new: CREATED!
    updated: UPDATED!
    failed: ERR!
    destory: DELETE!
    login: LOGIN!
    logout: LOGOUT!
    limited_access: UNAUTHORIZED!
    tag_follow: FOLLOWED!
    tag_not_follow: COUDNT FOLLOWED!
    tag_remove_follow: UN FOLLOWED!
```

###### active record

```app/models/client.rb
class Client < ApplicationRecord
  has_one :address
  has_many :orders
  has_and_belongs_to_many :roles
end

class Address < ApplicationRecord
  belongs_to :client
end

class Order < ApplicationRecord
  belongs_to :client, counter_cache: true
end

class Role < ApplicationRecord
  has_and_belongs_to_many :clients
end

```


```.rb
clients = Client.find(10)


```

```
```

###### RoRqr

```sh
vi Gemfile
+ gem 'rqrcode'
bundle install

./bin/setup
bundle exec rspec
./gin/console
```

```qr/basic.rb
require 'rqrcode'

qr = RQRCode::QRCode.new('http://github.com')
# qrcode = RQRCodeCore::QRCode.new('REGISTERION TKGCCI', size: 1, level: :m, mode: alphanumeric)
result = ''

qr.qrcode.modules.each do |row|
  row.each do |col|
    result << (col ? 'X' : 'O')
  end
  
  result << "\n"
end

puts result


# console
require 'rqrcode'

qr = RQRCode::QRCode.new('http://tkgcci.com', size: 4, level: :h)

puts qr.to_s

```


```qr/svg.rb
require 'rqrcode'
qrcode = RQRCode::QRCode.new("http://github.com/")

svg = qrcode.as_svg(
  offset: 0,
  color: '000',
  shape_rendering: 'crispEdges',
  module_size: 6,
  standalone: true
)

```

```qr/ansi.rb
require 'rqrcode'
qrcode = RQRCode::QRCode.new("http://github.com/")

svg = qrcode.as_ansi(
  light: "\033[47m", dark: "\033[40m",
  fill_character: '  ',
  quiet_zone_size: 4
)
```

```qr/png.rb
require 'rqrcode'

qrcode = RQRCode::QRCode.new("http://github.com/")

png = qrcode.as_png(
  bit_depth: 1,
  border_modules: 4,
  color: 'black',
  file: nil,
  fill: 'white',
  module_px_size: 6,
  resize_exactly_to: false,
  resize_gte_to: false,
  size: 120
)

IO.binwrite("/tmp/github-qrcode.png", png.to_s)
```




###### guard
---



```sh
gem install guard --no-ri --no-doc
gem install guard-rspec --no-ri --no-doc
guard init rspec

guard start

piccolo e 
```

```Guardfile
guard :rspec do
  watch(%r{^spec/.+_spec\.rb$})
  watch(%r{^lib/(.+)\.rb$})    { |m| "spec/lib/#{m[1]}_spec.rb" }
  watch('spec/spec_helper.rb') { "spec" }
  
  watch(%r{^app/(.+)\.rb$})                   { |m| "spec/#{m[1]}_spec.rb" }
  watch(%r{^app/(.*)(\.erb|\.haml|\.slim)$})  { |m| "spec/#{m[1]}#{m[2]}_spec.rb" }
  watch(%r{^app/controllers/(.+)_(controller)\.rb$}) { "spec" }
  watch(%r{^spec/support/(.+)\.rb$}) { "spec/routing" }
  watch('config/routes.rb') { "spec/routing" }
  watch('app/controllers/application_controller.rb') { "spec/controllers" }
  
  watch(%r{^app/views/(.+)/.*\.(erb|haml|slim)$}) { |m| "spec/features/#{m[1]}_spec.rb"}
  
  watch(%r{^app/views/(.+)/.*\.(erb|haml|slim)$})
  watch(%r{^spec/acceptance/steps/(.+)_steps\.rb$}) { |m| Dir[File.join("**/#{m[1]}.feature")][0] || 'spec/acceptance' }
end

guard :rspec do
  watch(%r{^spec/.+_spec\.rb$})
  watch(%r{^lib/(.+)\.rb$}) { |m| "spec/#{m[1]}_spec.rb" }
  watch('spec/spec_helper.rb') { "spec" }
end
```

##### geocoder

```rb
results = Geocoder.search("Osaka")
results.first.coordinates
# => []

results = Geocoder.search([48.856614, 2.3522219])
results.first.address
# => ""

results = Geocoder.search("192.168.11.xx")
results.first.coordinates
# => []
results.first.country
# => ""


```

```app/models/model.rb
class Model < ApplicationRecord
  def current_position
    # current spot
  end

  def address
    [street, city, state, country].compact.join(', ')
  end
  
  # lat, lon
  geocoded_by :address, latitude: :lat, longitude: :lon #ActiveRecord
end



```


```.rb
obj.distance_to([43.9, 98.6])
obj.bearing_to([43.9, 98.6])
obj.bearing_from(obj2)

```

```app/models/spot.rb
class Spot < ApplicationRecord
  geocoded_by :address
  after_validation :geocode
end

Spot.create(:address => "Dotonbori")
# => 

spot1 = Spot.find(1)
spot2 = Spot.find(2)
spot1.distance_to(spot2)
spot1.distance_from([xx.xxxxxx, xxx.xxxxxx])

Spot.near("Namba")
spot = Spot.find(1)
spot.nearbys(5, units: :km)

spot1 = Spot.find(1)
spot2 = Spot.find(2)
Geocoder::Calculations.geographic_center([spot1, spot2])
```


```sql
SELECT spots.*, (xx.xxxxx * ABS(spots.latitude - xx.xxx) * x.xxxxx) + (xx.xxx * ABS(spots.longitude - xxx.xxxx) * x.xxxx) AS distance, CASE WHEN (spot.latitude >= xx.xxxxxx AND spots.longitude >= xxx.xxxx) THEN xx.x WHEN (spots.latitude < xx.xxxx AND spots.longitude >= xxx.xxx) THEN xxx.x WHEN (spots.latitude < xx.xxx AND spots.longitude < xxx.xxx) THEN xxx.x WHEN (spots.latitude >= xx.xx AND spots.longitude < xxx.xxx) THEN xxx.x END AS bearing FROM "spots" WHERE (spots.latitude BETWEEN xx.xxxx AND xx.xxxx AND spots.longitude BETWEEN xxx.xxx AND xxx.xxxx) ORDER BY distance ASC LIMIT ?
```

```sh
rails g geocoder:config
```


```geocoder.rb
Geocoder.configure(
  lookup: :google,
  api_key: '--- YOUR_API_KEY---',

  units: :km
)

```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```

