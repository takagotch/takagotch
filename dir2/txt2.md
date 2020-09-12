###### ActiveModel::Attr
---


```active_record.rb
class Money < ApplicationReord; end

id = 'xxx'
money = Money.new(id: 'xxx')
money.id # => xxx
```

```active_record_attr.rb
class Money < ApplicationRecord
  attribute :time_zone, :string, default: -> { Time.zone.name }
  attribute :confirmed, :boolean, default: false
end

money = Money.new
Money.time_zone == Time.zone.name
money.confirmed == false
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

