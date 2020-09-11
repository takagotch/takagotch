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
  ioc: USA
  gec: US
  latitude: 38 00 N
  longitude: 97 00 W
  name: United States
  names:
  - The States
  national_destination_code_lengths:
  - 3
  national_number_lengths:
  - 10
  national_prefix: '1'
  number: '840'
  region: Americas
  subregion: Northern America
  world_region: AMER
  un_locode: US
  languages:
  - en
  translations:
    en: 'The United States of America'
    de: 'Vereinigte Staaten von Amerika'
    
  nationality: American
  postal_code: true
  min_longitude: "-179.231086"
  max_longitude: "-66.885417"
  max_latitude: '71.441059'
  latitude_dec: '39.44325637817383'
  longitude_dec: "-98.95733642578125"
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```


