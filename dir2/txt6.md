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

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

