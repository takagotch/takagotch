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

```active_model.rb
class RegisterationForm
  include ActiveModel::Model
  
  attr_accessor :accepted
  
  def accepted=(value)
    @accepted = ActiveModel::Type.lookup(:boolean).cast(value)
  end
  
  def accepted=(value)
    @accepted = value == 'xxx'
  end
  
end
```

```active_model_attr.rb
class RegistrationForm
  include ActiveModel::Model
  include ActiveModel::Attributes
  
  attribute :accepted, :boolean, default: false
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

money [send|receive].rb [tx|rx]
```active_record.rb
class User < ActiveRecord::Base
  has_many :money_sendings
  has_many :money_receivings
end

class Money < ActiveRecord::Base
end

class Money < ActiveRecord::Base
  belongs_to :User
  belongs_to :Money
end

class MoneyRecieving < ActiveRecord::Base
  belongs_to :User
  belongs_to :Money
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

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

