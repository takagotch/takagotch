###### Ransack, searchcop
---


```sh
rails new search_field
cd search_field
vi Gemfile
+ gem 'ransack'
bundle install --path vendor/bundle
bundle exec rails g model department name
budnle exec rails g model subject name
bundle exec rails g model student name sex age:integer department:references
budnle exec rails g model student_subject student:references subject:references

bundle exec rails db:migrate




```

```departmetn.rb
class Department < ApplicationRecord
  has_many :students
end
```

```subject.rb
class Subject < ApplicationRecord
  has_many :student_subjects
  has_many :student, through: :student_subjects
end
```

```student.rb
class Student < ApplicationRecord
  belongs_to :department
  has_many :student_subjects
  has_many :subjects, through: :student_subjects
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



