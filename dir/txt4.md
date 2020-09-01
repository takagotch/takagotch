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

```Gemfile
group :development, :test do
  gem 'bullet'
end

```

```sh
bundle install

curl http://localhost:3000/
```

```development.rb
config.after_initialize do
  Bullet.enable = true
  Bullet.alert = true
  Bullet.bullet_logger = true
  Bullet.console = true
  Bullet.rails_logger = true
  Bullet.add_footer = true
end
```

```student_controller.rb
class StudentsController < ApplicationController
  def index
    @students = @q.result.includes(:department, :subjects)
  end
  
  def search
    @q = Student.search(search_params)
    @students = @q.result.includes(:department, :subjects)
  end
  
end
```

```_search_form.html.slim
= search_form_for(@q, url:search_path) do |f|
  p
    = f.label :name_cont, 'NAME'
    = f.search_field :name_cont
  = f.submit
```

```students_controller.rb
def search_params
  params.require(:q).permit(:name_cont)
end
```

```student_controller.rb
def index 
  @q = Student.ransack(params[:q])
  @departments = Department.all
  @students = @q.result.includes(:department, :subjects)
end
```

```_search_form.html.slim
= search_form_for(@q, url:search_path) do |f|
  p
    = f.label :dpartment_id_eq, 'DEPARTMENT'
    = f.collection_select :department_id_eq, @departments, :id, :name, :include_blank => 'NO CATEGORRY'
  = f.submit    
```

```students_controller.rb
def search_params
  params.require(:q).permit(:department_id_eq)
end
```

```_search.html.slim
= search_form_for(@q, url:search_path) do |f|
  p
    = f.label :department_id_eq, 'DEPARTMENT'
      = f.radio_button 'department_id_eq', '', {:checked => true}
      | NO CATEGORY
    = f.collection_radio_buttons :department_id_eq, @departments, :id, :name
  = f.submit
```

```search.html.erb
= search_form_for(@q, url:search_path) do |f|
  p
    = f.label :sex_eq, 'SEX'
    = f.radio_button :sex_eq, '', {:checked => true}
    | NO CATEGORY
    = f.radio_button :sex_eq, 'MALE'
    | MALE
    = f.radio_button :sex_eq, 'FEMALE'
    | FEMALE
= f.submit
```

```search.html.slim
= search_form_for(@q, url:search_path) do |f|
  p
    = f.label :age_gteq, 'AGE'
    = f.radio_button 'age_gteq', '',{:checked => true}
    | NO CATEGORY
    = f.radio_button 'age_gteq', '15'
    | AGE 15 more
    = f.radio_button 'age_gteq', '20'
    | AGE 20 more
  = f.submit
```

```students_controller.rb
def index
  @q = Student.ransack(params[:q])
  @subjects = Subjects.all
  @studets = @q.result.includes(:department, :subjects)
end
```

```search.html.slim
= search_form_for(@q, url:search_path) do |f|
  p
    = f.label :subjects_id_in, 'SUBJECTS'
    = f.collection_check_boxes :subjects_id_in, @subjects, :id, :name
  = f.submit
```

```_search.html.slim
h1
  | SEARCH QUERY
- unless params[:q][:name_cont].empty?
  | NAME:
  = params[:q][:name_cont]
  br
- unless params[:q][:department_id_eq].empty?
  | DEPARTMENT:
  = Department.find(params[:q][:department_id_eq]).name
  br
- unless params[:q][:sex_eq].empty?
  | SEX:
  = params[:q][:sex_eq]
  br
- unless params[:q][:sex_eq].empty?
  | AGE:
  = params[:q][:age_gteq]
  | AGE MORE
  br
- unless params[:q][:subjects_id_in].reject(&:blank?).empty?
  | SUBJECTS:
  = params[:q][:subject_id_in].reject(&:blank?).map{|subject_id| Subject.find(subject_id).name}.join(', ')
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```



