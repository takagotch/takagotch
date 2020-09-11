###### countries
---



```lib/countries/data/countries
require 'yaml'; Dir['*'].map { |f| y = YAML.load(File.read(f)); data = y.to_a.last.last; [data["continent"], data["name"]]}
```

```data.yml
---
US:
  address_format: |-
    {{recipient}}
    {{street}}
    {{city}} {{region_short}} {{postalcode}}
    {{country}}
  alpha2: US
  alpha3: USA
  continent: North America
  country_code: '1'
  currency: USD
  international_prefix: '011'
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```


