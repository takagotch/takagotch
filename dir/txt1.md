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


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


