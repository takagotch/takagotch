###### vue, coffee, gon
---


```
```

```
```
###### kaminari

```Gemfile
gem 'kaminari', '~> 0.17.0'

gem 'kaminari-bootstrap', '~> 3.0.1'
```

```sh
bundle install
```

```app/controllers/home_controller.rb
class HomeController < ApplicationController
  PER = 8
  
  def index
#   @words = Word.all
    @words = Word.page(params[:page]).per(PER)
  end
end


```

```app/views/home/index.html.erb
<div class="page-header">
  <h1>WORD LIST</h1>
</div>
<div class="list-group">
  <% @words.each do |word| %>
    <%= link_to(word, class: 'list-group-item') do %>
      <h4 class="list-group-item-heading">
        <%= word.word %>
      </h4>
      <p class="list-group-item-text">
        <%= word.reading %>
      </p>
    <% end %>
  <% end %>
  <%= paginate @words %>
</div>
```

```config/locales/kaminari_ja.yml
ja:
  views:
    pagination:
      first: "&laquo; HOME"
      last: "END &raquo;"
      previous: "&lsaquo; PREV"
      next: "NEXT &rsaquo;"
      truncate: "..."
```

```
```

###### Active Record insert_all
```.rb
row_ticket_sales = Ticket.joins(:reviews, :sales).where(some_conditions).select(some_columns).group("tickets.id")

ticket_sales = row_ticket_sales.map do |sales|
  sales_last_month = SalesLastMonth.new
  [:some, :columns, :name].each do |column|
    sales_last_month.send("#{column.to_s}=", sales.send(column))
  end
  sales_last_month
end
SalesLastMonth.import ticket_sales
```

```
```

###### paranoia

```sh
vi Gemfile
+ gem 'paranoia'
./bin/rails g model user name age:integer deleted_at:datetime
./bin/rails db:migrate


```

```app/models/user.rb
class User < ApplicationRecord
  acts_as_paranoid column: :destroyed_at
# acts_as_paranoid without_default_scope: true
end

User.where( 'age > ?', 20)
user = User.find(1)
user.destroy
user.deleted_at
User.where('age > ?', 20)
user.really_destroy!

User.where('age > ?', 20)
user.destroy

User.all
User.with_deleted
User.without_deleted

User.where('age > ?', 20).with_deleted
User.only_deleted

user = User.only_deleted.first
user.deleted?
user.paranoia_destroyed?

user.restore
User.restore(2)
```

```sh
rails g migration AddDeletedAtToUsers deleted_at:datetime:index
rails db:migrate

rails c
user = User.find(1)
user.destory
user = User.find(1)
user.really_destroy!
User.only_deleted
User.only_deleted.first
User.only_deleted.find(4)
User.with_deleted
User.without_deleted
user.restore
User.restore(2)
```

```app/models/user.rb
class AddDeletedAtToUsers < ActiveRecord::Migration
  acts_as_paranoid
  
  def change
    add_column :users, :deleted_at, :datetime
    add_index :users, :deleted_at
  end
end

class Client < ActiveRecord::Base
  acts_as_paranoid column: :destroyed_at
# acts_as_paranoid without_default_scope: true
end

class User < ActiveRecord::Base
  acts_as_paranoid
  
  after_destroy :update_document_in_search_engine
  after_restore :update_document_in_search_engine
  after_real_destroy :remove_document_from_search_engine

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

