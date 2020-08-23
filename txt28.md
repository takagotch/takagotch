##### Active_model_serializers
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

##### fast_jsonapi
```sh
rspec
rails new fast_json_api --api
vi Gemfile
+ gem 'fast_jsonapi'

```

```recipe.rb
class Recipe < ApplicationRecord
  has_many :ingredients, dependent: :destroy
end

```

```ingredient.rb
class Ingredient < ApplicationRecord
  belongs_to :recipe
end

```

```.rb
create_table :recipes do |t|
end
create_table :ingredients do |t|
  t.references : recipe, null: false
  t.string :name, null: false
  t.string :quantity_and_unit, null: false
  t.timestamps
end

```

```output.rb
Recipe.all.each do |recipe|
  ingredients.each_with_index do |array, index|
    name = array[0]
    quantity_and_unit = array[1]
    recipe.ingredients.create({
      name: name,
      quantity_and_unit: quantity_and_unit
    })
  end
end
```

```.rb
class Api::V1::RecipesController < ApplicationController
end
```

```.sh
rails g serializer recipe
```

```recipe.rb
class RecipeSerializer
  include FastJsonapi::ObjectSerializer
  attributes
end

class RecipeSerializer
  include FastJsonapi::ObjectSerializer
  has_many :ingredients
  attributes :title, :introduction
end
```
```ingredient.rb
class IngredientSerializer
  include FastJsonapi::ObjectSerializer
  belongs_to :recipe
  attributes :name, :quantity_and_unit
end
```
```recipe_controller.rb
class Api::V1::RecipeController < ApplicationController
  def show
    recipe = Recipe.find(params[:id])
    options = { include: %i[ingredients] }
    json_string = RecipeSerializer.new(recipe).serialized_json
    render json: json_string
  end
end
```
```output
{
  "data": {
    "id": "1",
    "type": "recipe",
    "attributes": {
      "title": "texttext...",
      "introduction": "texttext...",
    },
    "": {}
  },
  "included": []
}
```

```recipe.rb
class RecipeSerializer
  include FastJsonapi::ObjectSerializer
  cache_options enabled: true, cache_length: 1.hours
  has_many :ingredients
  attributes :title, :introduction
end

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

###### jsonapi-resources
```contacts_controller.rb
class ContactsController < ApplicationController
  include JSONAPI::ActsAsResourceController
  
  protect_from_forgery with: :null_session
end

class ContactResource < JSONAPI::Resource
  attributes :name_first, :name_last, :email, :twitter
  attribute :name_full
  
  has_many :phone_numbers
  
  def name_full
    "#{name_first} #{name_last}"
  end
end

class Contact < ActiveRecord::Base
   has_many :phone_numbers
   
   validates :name_first, presence: true
   validates :name_last, presence: true
end
```
######
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```


