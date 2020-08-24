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

###### grape::formatter::roar
```.rb
class API < Grape::API
  format :json
  formatter :json, Grape::Formatter::Roar
end

module ProductRepresenter
  include Roar::JSON
  include Roar::Hypermedia
  include Grape::Roar::Representer
  
  property :title
  property :id
end

get 'product/:id' do
  present Product.find(params[:id]), with: ProductRepresenter
end

module ProductsRepresenter
  include Roar::JSON::HAL
  include Roar::Hypermedia
  include Grape::Roar::Representer
  
  collection:entries, extend: ProductRepresenter, as: :products, embedded: true
end

class ProductRepresenter < Grape::Roar::Decorator
  include Roar::JSON
  include Roar::Hypermedia
  
  link :self do |opts|
    "#{request(opts).url}/#{represented.id}"
  end
  
  private
  
  def request(opts)
    Grape::Request.new(opts[:env])
  end
end

get 'products' do
  present Product.all, with: ProductsRepresenter
end

```

```model.rb
class Item < ActiveRecord::Base
  belongs_to :cart
end

class Cart < ActiveRecord::Base
  has_many :items
end

```

```.rb
class ItemEntity < Grape::Roar::Decorator
  include Roar::JSON
  include Roar::JSON::HAL
  include Roar::Hypermedia
  
  include Grape::Roar::Extensions::Relations
  
  relation :belongs_to, :cart, embedded:true
  
  link_self
end

class CartEntity < Grape::Roar::Decorator
  include Roar::JSON
  include Roar::JSON::HAL
  include Roar::Hypermedia
  
  include Grape::Roar::Extensions::Relations
  
  relation :has_many, :items, embedded: false
  
  link_self
end
```

```output
{
  "_embedded": {
    "cart": {
      "_links": {
        "self": {
          "href": "http://tkgcci.com/carts/1"
        },
        "items": [{
          "href": "http://tkgcci.com/items/1"
        }]
      }
    }
  },
  "_links": {
    "self": {
      "href": "http://example.org/items/1"
    }
  }
}

{
  "": 
}
```

```adapter.rb
module Extensions
  module RelationalModels
    module Adapter
      class ActiveRecord < Base
        include Validations::ActiveRecord
        
        valid_for { |klass| klass < ::ActiveRecord::Base }
        
        def collection_methods
          @collection_metohds ||= %i(has_many has_and_belongs_to_many)
        end
        
        def name_for_represented(represented)
          klass_name = case represented
                       when ::ActiveRecord::Relation
                         represented.klass.name
                       else
                         represented.class.name
                       end
          klass_name.demodulize.pluralize.downcase             
        end
        
        def single_entity_methods
          @single_entity_methods ||= %i(has_one belongs_to)
        end
      end
    end
  end
end
```

```validateions.rb
module ActiveRecord
  include Validations::Misc
  
  def belongs_to_valid?(relation)
    relation = klass.reflections[relation]
    
    return true if relation.is_a?(
      ::ActiveRecord::Reflection::BelongsToReflection
    )
    
    invalid_relation(
      ::ActiveRecord::Reflection::BelongsToReflection,
      relation.class
    )
  end
end
```

```urimapping.rb
class BarEntity < Grape::Roar::Decorator
  include Roar::JSON
  include Roar::JSON::HAL
  include Roar::Hypermedia
  
  include Grape::Roar::Extensions::Relations
  
  map_resource_path do |_opts, object, relation_name|
    "#{relation_name}/#{object.id}"
  end
  
  relation :has_many, :bars, embedded: false
end

class BarEntity < Grape::Roar::Decorator
  include Roar::JSON
  include Roar::JSON::HAL
  include Roar::Hypermedia
  
  include Grape::Roar::Extensions::Relations
  
  map_base_url do |opts|
    request = Grape::Request.new(opts[:env])
    "#{request.base_url}#{request.script_name}"
  end
  
  relation :has_many, :bars, embedded: false
end

Grape::Roar::Extensions::Relations::Exceptions::InvalidRelationError:
  Expected Mongoid::Relations::Referenced::One, got Mongoid::Relations::Referenced::Many!
```

##### jbuilder
```index.json.jbuilder
json.text "TEXT"
json.set! :text, "TEXT"



```

```posts_controller.rb
class Api::PostsController < ApplicationController

  def index
    @posts = Post.all
  end

  def show
    @post = Post.find(params[:id])
  end
end
```

```show.json.jbuilder
json.post do
  json.merge! @post.attributes
end
```

```.rb
json.total_pages @events.total_pages

# =>
#
#
# {
#   "total_pages": 10
#  }
#

if @event.image.present?
end

json.finished @event.finished?
class Event < ApplicationRecord
  def finished?
    return true if event_dates.blank?
    event_dates.last.date < Date.current
  end
end

json.array!(events) do |event|
  if group_event == event
    next
  end
  json.id event.id
end


```

##### grape, jbuilder
```app/api/article.rb
class API < Grape::API
  format :json
  formatter :json, Grape::Formatter::Jbuilder
  
  get '/', jbuilder: 'article/index' do
    @articles = Article.all
  end
end
```

```sh
vi Gemfile
+ gem 'jbuilder'
+ gem 'grape'
+ gem 'grape-jbuilder'
bundle install


```

```app/views/api/article/index.jbuilder
json.articles @articles do |article|
  json.(article, :id, :title)
end
```

```result
{
  articles: [
    {
      "id": 1,
      "title": "TITLE1",
    },
    {
      "id": 2,
      "title": "TITLE2",
    }
  ]
}
```

```config/application.rb
module Extensions
  module RelationalModel
    module adapter
      class Application < Rails::Application
        config.paths.add "", glob: "**/*.rb"
        config.autoload_paths += %W(#{config.root}/app/api)
        
        config.middleware.use(Rack::Config) do |env|
          env['api.tilt.root'] = Rails.root.join 'app', 'views', 'api'
        end
      end
    end
  end
end
```

```app/views/messages/show.json.jbuilder
json.content format_content(@message.content)
json.(@message, :created_at, :updated_at)

json.author do
  json.name @message.creator.name.familiar
  json.email_address @message.creator.email_address_with_name
  json.url url_for(@message.creator, format: :json)
end

if current_user.admin?
  json.visitors calculate_visitors(@message)
end

json.comments @message.comments, :content, :created_at

json.attachments @message.attachments do |attachment|
  json.filename attachment.filename
  json.url url_for(attachment)
end

```

```result
{
  "content": "<p>This is <i>serious</i> monkey business</p>",
  "": "",
  "": "",
  "": {},
  
  "": 15,
  "": [],
  
  "attachments": [
    { "filename": "forcast.xls", "url": "http://tkgcci.com/downloads/forecast.xls" },
    { "filename": "presentation.pdf", "url": "http://tkgcci.com/downloads/presentation.pdf" }
  ]
}
```

```.rb
json.set! :author do
  json.set! :name, 'tky'
end
# => {"author": { "name": "tky" }}

hash = { author: { name: "tky" } }
json.post do
  json.title "Merge HOWTO"
  json.merge! hash
end

# => "post": { "title": "Merge HOWTO", "author": { "name": "tky" }}

```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```



