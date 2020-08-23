###### Active_model_serializers
---

```sh
rails g serializer books
```

```book.rb
# Table name: books
#
# id           :integer
# title        :string(255)
# price        :integer
# published_at :datetime
# publisher_id :integer
# created_at   :datetime
# created_at   :datetime

class Book < ActiveRecord::Base
  belongs_to: publisher
  has_and_belongs_to_many :auhtors
end

```

```book_serializer.rb
class BookSerialize < ActiveModel::Serializer
  attributes :id, title, :price, published_at, :publisher_id
  has_many :authors
  
  def url
    book_url(object)
  end
  
  def include_url?
    self.title =~ /TITLE/
  end
  
  def authors
    object.authors.where(name: 'tky')
  end
  
end
```

```books_controller.rb
def index
  @books = Book.all
  respond_to do |format|
    format.html
    format.json { render json: @books } #root: false
  end
end

```

```output
{"books":[{"id":1, "title":"TITLE", "price":100, "publishered_at":null, "publisher_id":1},
{"id":2,"title":"TITLE", "price":200, "published_at":null, "publisher_id":2}]}
```
```
```
```
```

###### rack tracker, rack google analytics

```rack-tracker.rb
config = MyApplication::Application.config
config.middleware.use(Rack::Tracker) do
  handler :google_analytics, { tracker: 'U-xxxx-Y' }
end

tracker do |t|
  t.google_analytics :send, { type: 'event', category: 'CATEGORYNAME', action: 'ACTIONNAME', label: 'LABELNAME', value: 'VALUE' }
end

def show
  tracker do |t|
    t.google_analytics :send, { type: 'event', category: 'button', action: 'click', label: 'nav-buttons', value: 'X' }
  end
end

```

######
```
```

```
```


