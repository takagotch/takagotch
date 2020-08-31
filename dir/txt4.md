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

bundle exec rails db:seed
bundle exec rails g controller students index search

bundle exec rails s
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

```seeds.rb
Department.create()
Department.create()
Department.create()
Department.create()
Subject.create(name:'')
StudentSubject.create(student_id:1, subject_id:1)
StudentSubject.create(student_id:1, subject_id:2)
```

```students_controller.rb
class StudentController < ApplicationController
  def index
    @q = Student.ransack(params[:q])
    @students = @q.result(distinct: true)
  end
  
  def search
    @q = Student.search(search_params)
    @students = @q.result(distinct: true)
  end
  
  private
  def search_params
    params.require(:q).permit!
  end
end

```

```routes.rb
Rails.application.routes.draw do
  root to: 'students#index'
  get 'search', to: 'students#search'
end
```

```view/students/index.html.slim
h1
  | STUDENTS SEARCH
= render 'search_form'
table
 - @students.each do |student|
   tr
     td
       = student.name
     td
       = student.sex
     td
       = student.age
     td 
       = student.department.name
     td 
       = student.subjects.map{|subject_id| Subject.find(subject_id).name}.join(', ')

```

```views/students/_search_form.html.slim
= search_form_for(@q, url:search_path) do |f|

= f.submit
```

```view/students/search.html.slim
h1
  | SEARCH RESULT
table 
 - @students.each do |student|
   tr
     td 
       = student.name
     td
       = student.sex
     td 
       = student.age
     td 
       = student.department.name
     td
       = student.subjects.map{|subject_id| Subject.find(subject_id).name}.join(', ')
= link_to 'TOP', root_path              
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```



