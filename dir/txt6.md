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

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

