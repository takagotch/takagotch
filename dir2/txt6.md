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

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

