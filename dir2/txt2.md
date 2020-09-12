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

!()[]
```
ER図
```


scope
```
class User < ActiveRecord::Base
  scope :keyword_search, ->(keyword) do
    where(<<-SQL, money_name: keyword.downcase, comment: "%#{keyword}")
    --
    (
      EXISTS (
        SELECT *
        FROM money_sendings mtx
        INNER JOIN moneys m
        ON m.id = mtx.money_id
        WHERE
        moneys.id = mtx.money_id
        AND LOWER(m.name) = :money_name
      )
    ) OR
    --
    (
      EXISTS (
        SELECT *
        FROM money_receivings mrx
        INNER JOIN moneys m
        ON m.id = mrx.money_id
        WHERE
        users.id = mrx.user_id
        AND LOWER(m.name) = :money_name
      )
    ) OR
    --
    moneys.comment USERSEND :comment
  SQL
    end
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

```usage.rb
class Entries::Form
  include ActiveModel::Model
  include ActiveModel::Attributes
  
  attribute :title, :string
  attribute :body, :string
  attribute :draft, :boolean, default: false
  attribute :published_at, :datetime
  attribute :tags
  
  attribute :published_at, :datetime, default: ->{ Time.now }
  
  def initialize(user, entry, params = {})
    @user = user
    @entry = entry
    super(params)
  end
  
  @attributes = self.class._default_attributes.deep_dup
  assign_attributes(params)
  
  form.published_at = "2020/09/13 02:01"
  form.published_at # => 2020-09-13 02:01:00 +0900
  form.published_at = "xxxx"
  form.published_at # => nil
  
  form.draft = "1"
  form.draft # => true
  form.draft = "0"
  form.draft # => false
  
  form.attributes
  # => {"title"=>"TITLE", "body"=>"BAR", "daft"=>false, "published_at"=>2020-09-13 02:02:00 +0900, "tags"=>["Tx", "Rx"]}
  
  form.attribute_names
  # => ["title", "body", "draft", "published_at", "tags"]
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

