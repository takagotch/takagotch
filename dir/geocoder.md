###### geocoder
---


```config/initializers/geocoder.rb
if %w(development test).include? Rails.env
  module Geocoder
    module Request
      def geocoder_sppofable_ip_with_localhost_override
      end
      
    # alias_method_chain is deprecated
    # alias_method_chain :geocoder_spoofable_ip, :localhost_override
    
      alias_method :geocoder_spoofable_ip_without_localhost_override, :geocoder_spoofable_ip
      alias_method :geocoder_spoofable_ip, :geocoder_spoofable_ip_with_localhost_override
    end
  end
end

```

```usage.rb
results = Geocoder.search("Paris")
results.first.coordinates
# => [xx.xxxxxx, x.xxxxxxx ]

results = Geocoder.search([xx.xxxxxx, x.xxxxxxx])
results.first.address

results = Geocoder.search("xxx.xx.xx.xx")
results.first.coordinates
# => [xx.xxxxxx, -xx.xxxxxxx]
results.first.country
# => "United States"



```


```objects.rb

def address
  [street, city, state, country].compact.join(', ')
end

geocoded_by :address

after_validation :geocode



```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```

####### google-map-for-rails

```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```

