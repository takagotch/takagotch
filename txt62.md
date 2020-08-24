###### 
---



```


```

```
```

```
```


```
```

```
```

```
```


```
```

```
```

```
```


```
```

```
```

```
```

###### simple form
```app/views/usage.html.erb
<%= simple_form_for @user do |f| %>
  <%= f.input :username, label: 'Your username please', error: 'Usaername is mandatory, please specify one' %>
  <%= f.input :password, hint: 'No special characters.' %>
  <%= f.input :email, placeholder: 'user@domain.com' %>
  <%= f.input :remember_me, inline_label: 'Yes, remember me' %>
  <%= f.button :submit %>
<% end %>

<%%>

```

```app/models.rb
class User < ActiveRecord::Base
  belongs_to :company
  has_and_belongs_to_many :roles
end

class Company < ActiveRecord::Base
  has_many :users
end

class Role < ActiveRecord::Base
  has_and_belongs_to_many :users
end

```

```.rb
<%= simple_form_for @user do |f| %>
  <%= f.input :name %>
  <%= f.association :company %>
  <%= f.association :roles %>
  <%= f.button :submit %>
<% end %>
```

###### simple form, inputs
```app/inputs/currency_input.rb
class CurrencyInput < SimpleForm::Inputs::Base
  def input(wrapper_options)
    merged_input_options = merge_wrapper_options(input_html_options, wrapper_options)
    
    "$ #{@builder.teext_field(attribute_name, merged_input_options)}".html_safe
  end
end

```

```app/views/inputs/show.html.erb
f.input :money, as: :currency
```

```app/inputs/date_time_input.rb
class DateTimeInput < SimpleForm::Inputs::DateTimeInput
  def input(wrapper_options)
    template.content_tag(:div, super)
  end
end

```

```app/inputs/collection_select_input.rb
class CollectionSelectInput < SimpleForm::Inputs::CollectionSelectInput
  def input_html_classes
    super.push('chosen')
  end
end

```

```app/inputs/custom_inputs/numeric_input
module CustomInputs
  class NumericInput < SimpleForm::Inputs::NumericInput
    def input_html_classes
      super.push('no-spinner')
    end
  end
end

```

```config/simple_form.rb
config.custom_inputs_namespaces << "CustomInputs"

```

```i18n.yml
ja:
  simple_form:
    label:
      user:
        username: 'Tky'
        password: 'password'
      hints:
        user:
          username: 'User name to sign in.'
          password: 'No special characters, please.'
      placeholders:
        user:
          username: 'tky'
          password: '****'
      include_blanks:
        user:
          age: 'Rather not say'
      prompts:
        user:
          role: 'Select your role'

```
###### wrappers api
```.rb
config.wrappers tag: :div, class: :input,
                error_class: :field_with_errors,
                valid_class: :field_without_errors do |b|
  b.use :html5
  b.optional :pattern
  b.use :maxlength
  b.use :placeholder
  b.use :readonly

  b.use :label_input
  b.use :hint, wrap_with: { tag: :span, class: :hint }
  b.use :error, wrap_with: { tag: :span, class: :error }
end
```

```

```

```

```

