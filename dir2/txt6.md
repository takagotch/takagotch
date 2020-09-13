###### active_model_serializers, jbuider
---
# => json

```project.rb
class Project < ApplicationRecord
  has_many :jobs, dependent: :destroy
  has_many :project_categories, dependent: :destroy
  has_many :categories, through: :project_categories
end


```

```job.rb
class Job < ApplicationRecord
  belongs_to :project
end
```

```project_category.rb
class ProjectCategory < ApplicationRecord
  belongs_to :project
  belongs_to :category
end
```

```.sh
rails g serializer model
```

```jobs_serializer.rb
class JobSerializer < ActiveModel::Serializer
  attributes :id, :name
end

```

```app/controllers/jobs_controller.rb
class Jobs Controller < ApplicationController
  
  
  def index
    @job = Job.first
    render json: @job, serializer: JobSerializer
   
    # n+1
    @jobs = Job.preload(:project)
    render json: @jobs, each_serializer: JobSerializer
    #
    Job.preload(project: [:categories])
    render json: @jobs, each_serializer: JobSerializer, include: { project: [ :categories] }
    # => [ { "": 1, ...}, "": "", "": { "": 1, "": "", "": [ { "": 1, "": "" }, {}, ...] } ]
  
  end
  
  # =>
  # {
  # "id": 1,
  # "name": "job1"
  # }
  
  
end

```

```projects_serializer.rb
class ProjectSerializer < ActiveModel::Serializer
  attributes :id, :name
end


```

```jobs_serializer.rb
class JobSerializer < ActiveModel::Serializer
  attributes :id, :name
  
  belongs_to :project, serializer: ProjectSerializer
end
```

```categories_serializer.rb
class CategorySerializer < ActiveModel::Serializer
  attributes :id, :name
  
  has_many :categories, serializer: CategorySerializer
end

```

```
```

###### jbuilder

```jbuilder.rb
json.(@message, :content, :author)
# => {content: 'hello', author: 'xxx'}
```

```active_model_serializer.rb
class MessageSerializer < ActiveModel::Serializer
  attributes :content, :author, :role
  
  def role
    object.role.name
  end
end

```

```call.rb
render json: @message
```

ActiveModel::Serializers
```users_controller.rb
def list_with_serializer
  users = User.all
  render json: ActiveModel::ArraySerializer.new(
    users,
    each_serializer: UserSerializer
  ).to_json
end

```

```user_serialiser.rb
class UserSerializer < ActiveModel::Serializer
  attributes :id, :email, :name
  
  def name
    object.display_name
  end
  
end

```

```.rb
class Post
  include ActiveModel::Serialization
  include ActiveModel::Model
end

ActiveModel::ArraySerializer.new(
  users,
  each_serializer: UserSerializer,
  message: message
)
```

jbuilder
```users_controller.rb
def list_with_jbuilder
  @users = User.all
  render :list_with_jbuilder
end
```

```list_with_jbuilder.jbuilder
json.array! @users.each do |user|
  json.partial! 'user', user: user
end
```

```_user.jbuilder
json.id user.id
json.email user.email
json.name user.display_name
```

```
```

```
```

```post_controller.rb
class PostsController < ApplicationController
  def show
    @post = Post.find(params[:id])
    render json: @post, serializer: PostSerializer
  end
end


```

```post_serializer.rb
class PostSerializer < ActiveModel::Serializer
  attribute :title
  
  attribute :created_at, key: :timestamp
  
  has_many :comments, serializer: CommentSerializer
  
  attribute :published
  
  def published
    object.published_at.present?
  end
end

class CommentSerializer < ActiveModel::Serializer
  attribute :body
end

{
  title: "TITLE",
  timestamp: "2020-09-14T00:57:00+0900",
  published: true,
  comments: [
    {
      body: "COMMENTS"
    }
  ]
}
```

```master_data_controller.rb
class MasterDataController
  render json: MasterData.new, serializer: MasterDataSerializer
end
```

```master_data.rb
class MasterData
  def first_master_data
    @first_master_data ||= Master::FirstMasterData.all
  end
  
  def second_master_data
    @second_master_data ||= Master::SecondMasterData.all
  end
end
```

```master_data_serializer.rb
class MasterDataSerializer
  has_many :first_master_data
  has_many :second_master_data
  
  class FirstMasterDataSerializer
    attributes :id, :name
  end
  
  class SecondMasterDataSerializer
    attributes :id, :name
  end
end

```

```
```

```app/controllers/post_controller.rb
class PostsController < ApplicationController
  def show
    @post = Post.find(params[:id]).preload(:comments)
    render json: @post, serializer: PostSerializer
  end
end
```

```app/serializers/posts_serializer.rb
class PostSerializer < ActiveModel::Serializer
  attribute :title
  
  has_many :comments, serializer: CommentSerializer
end

class CommentSerializer < ActiveModel::Serializer
  attribute :body
end
```

```
```

```app/controllers/posts_controller.rb
class PostsController < ApplicationController
  def show
    @post = Post.find(params[:id]).preload(:comments)
  end
end
```

```app/views/posts/show.json.jbuilder
json.title @post.title
json.page_views do
  json.partial! partial: 'comment', collection: @post.comments, as: :comment
end
```

```app/views/posts/_comments.json.jbuilder
json.body comments.body
```

```
```

```
```

```post_serializer.rb
class PostSerializer < ActiveModel::Serializer
  attribute :created_at, key: :timestamp
  attribute: title
  
  def title
    " this book title is #{obj.title}"
  end
  
end
```

```call.rb
render json: @users, root: 'data', each_serializer: User::ShowSerializer

@users.each do |user|
  User::ShowSerializer(user)
end

render json: @user, root: 'data', serializer: User::ShowSerializer

User::ShowSerializer(@user)


def call
  @post = Post.find(params[:id])
  render json: @post, serializer: PostSerializer
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

```secure_password.rb
class Person
  include ActiveModel::SecurePassword
  has_secure_password
  has_secure_password :recovery_password, validations: false
  attr_accessor :password_digest, :recovery_password_digest
end

person = Person.new

person.valid?

person.password = 'pass'
person.password_confirmation = 'pass'
person.valid?

person.password = 'pass'
person.valid?

person.password = person.password_confirmation = 'tky'
person.valid?

person.recovery_password = "pass"

person.authenticate('pass')
person.authenticate('pass')
person.authenticate_password('pass')
person.authenticate_password('pass')

person.authenticate_recovery_password('pass')
person.authenticate_recovery_password('pass')

person.password_digest
person.recovery_password_digest


```

```
```

```serialization.rb
class Person
  include ActiveModel::Serialization
  
  attr_accessor :name
  
  def attributes
    {'name' => nil}
  end
end

person = Person.new
person.serializable_hash
person.name = "tky"
person.serializable_hash

class Person
  include ActiveModel::Serializers::JSON
  
  attr_accessor :name
  
  def attributes
    {'name' => nil}
  end
end

person = Person.new
person.as_json
person.name = "Tky"
person.as_json

class Person
  include ActiveModel::Serializers::JSON
  
  attr_accessor :name
  
  def attributes=(hash)
    hash.each do |key, value|
      send("#{key}=", value)
    end
  end
  
  def attributes
    {'name' => nil}
  end
end

json = { name: 'tky' }.to_json
person = Person.new
person.from_json(json)
person.name
```

```app/models/person.rb translation.rb
class Person
  extend ActiveModel::Translation
  
  include ActiveModel::Model
  
end

Person.human_attribute_name('name')
```

```config/locales/app.pt-BR.yml
pt-BR:
  activemodel:
    attributes:
      person:
        name: 'Tky'
```

```test/models/person_test.rb
require 'test_helper'
  class PersonTest < ActiveSupport::TestCase
  include ActiveModel::Lint::Tests
  
  setup do
    @model = Person.new
  end
  end
end
```

```.sh
rails test
```

```
```

```validations.rb
class Person
  include ActiveModel::Validations
  
  attr_accessor :name, :email, :token
  
  validates :name, :email, :token
  validates_format_of :email, with: /\A([^\s]+)((?:[-a-z0-9]\.)[a-z]{2,})\z/i
  validates! :token, presence: true
end

person = Person.new(token: "xxx")
person.valid?
person.name = 'xxx'
person.email = 'xxx'
person.valid?
person.email = 'info@gmail.com'
person.valid?
person.token = nil
person.valid?
```

```naming.rb
class Person
  extend ActiveModel::Naming
end

Person.model_name.name
Person.model_name.singular
Person.model_name.plural
Person.model_name.element
Person.model_name.human
Person.model_name.collection
Person.model_name.param_key
Person.model_name.i18n_key
Person.model_name.route_key
Person.model_name.singular_route_key
```

``` ActiveModel::model.rb
class EmailContact
  include ActiveModel::Model
  
  attr_accessor :name, :email, :message
  validates :name, :email, :message, presence: true
  
  def deliver
    if valid?
    end
  end
end


email_contact = EmailContact.new(name: 'Tky',
                                 email: 'tky@gmail.com',
                                 message: 'Hello')
email_contact.name
email_contact.email
email_contact.valid?
email_contact.persisted?
```

```ActiveModel::Dirty.rb
require 'active_model'

class Person
  include ActiveModel::Dirty
  define_attribute_methods :first_name, :last_name
  
  def first_name
    @first_name
  end
  
  def first_name=(value)
    first_name_will_change!
    @first_name = value
  end
  
  def last_name
    @last_name
  end
  
  def last_name=(value)
    last_name_will_change!
    @last_name = value
  end
  
  def save
    changes_applied
  end
end


person = Person.new
person.changed?

person.first_name = "Takashi Yoshioka"
person.first_name

person.changed?

person.changed

person.changed_attributes

person.changes

person.first_name
person.first_name_changed?

person.first_name_was

person.first_name_change
person.last_name_change
```

``` conversion.rb
class Person
  include ActiveModel::Conversion
  
  def persisted?
    false
  end
  
  def id
    nil
  end
end

person = Person.new
person.to_model == person
person.to_key
person.to_param
```

``` ActiveModel::Callbacks.rb
class Person
  extend ActiveModel::Callbacks
  
  define_model_callbacks :update
  
  before_update :reset_me
  
  def update
    run_callbacks(:update) do
    end
  end
  
  def reset_me
  end
end


```

``` ActiveModel::AttributeMethods
class Person
  include ActiveModel::AttributeMethods
  
  attribute_method_prefix 'reset_'
  attribute_method_suffix '_highest?'
  define_attribute_methods 'age'
  
  attr_accessor :age
  
    private
    def reset_attribute(attribute)
      send("#{attribute}=", 0)
    end
    
    def attribute_highest?(attribute)
      send(attribute) > 100
    end
end

person = Person.new
person.age = 110
person.age_highest?
person.reset_age
person.age_highest?
```

```
```

```
```

```
```

```
```

```
```

```
```

