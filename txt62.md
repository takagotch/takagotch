###### nokogiri
---

```.rb
require 'nokogiri'
require 'open-uri'

doc = Nokogiri::HTML(URI.open('https://nokogiri.org/tutorials/installing_nokogiri.html'))

puts "### Search for nodes by css"
doc.css('nav ul.menu li a', 'article h2').each do |link|
end

puts "### Search for nodes by xpath"
doc.xpath('//nav//ul//li/a', '//article//h2').each do |link|
end

puts "### Or mix and match."
doc.search('nav ul.menu li a', '//article//h2').each do |link|
  puts link.content
end


```

```sh
bundle install
bundle exec rake compile test
```

```.rb
doc = Nokogiri.XML('<foo><bar /></foo>', nil, 'EUC-JP')
```

###### html-pipeline
```.rb
require 'html/pipeline'
filter = HTML::Pipeline::MarkdownFilter.new("Hi **world**!")
filter.call

pipeline = HTML::Pipeline.new [
  HTML::Pipeline::MarkdownFilter,
  HTML::Pipeline::SyntaxHighlightFilter
]
result = pipeline.call <<-CODE
This is *great*:

  some_code(:first)
  
CODE
result[:output].to_s

```

```rb
filter = HTML::Pipeline::MarkdownFilter.new("Hi **world**!", :gfm => false)
filter.call

context = {
  :asset_root => "http://tkgcci.com/where/your/images/live/icons",
  :base_url   => "http://tkgcci.com" 
}

SimplePipeline = Pipeline.new [
  SanitizationFilter,
  TableOfContentsFilter, 
  CamoFilter,
  ImageMaxWidthFilter,
  SyntaxHighlightFilter,
  EmojiFilter,
  AutolinkFilter
], context

MarkdownPipeline = Pipeline.new [
  MarkdownFilter,
  SanitizationFilter,
  CamoFilter,
  ImageMaxWidthFilter,
  HttpsFilter,
  MentionFilter,
  EmojiFilter,
  SyntaxHighlightFilter
], context.merge(:grm => true)

NonGFMMarkdownPipeline = Pipeline.new(MarkdownPipeline.filters, context.merge(:gfm => false))

HtmlEmailPipeline = Pipeline.new [
  PlainTextInputFilter,
  ImageMaxWidthFilter
], {}

EmojiPipeline = Pipeline.new [
  PlainTextInputFilter,
  EmojiFilter
], context
```

```extending.rb
require 'uri'

class RootRelativeFilter < HTML::Pipeline::Filter
  def call
    doc.search("img").each do |img|
      next if img['src'].nil?
      src = img['src'].strip
      if src.start_with? '/'
        img["src"] = URI.join(context[:base_url], src).to_s
      end
    end
  end
  
end

Pipeline.new [ RootRelativeFilter ], { :base_url => 'http://tkgcci.com' }
```


```instrumenting.rb
service = ActiveSupport::Notifications

pipeline = HTML::Pipeline.new [MarkdownFilter], context
pipeline.setup_instrumentation "MarkdownPipeline", srvice

HTML::Pipeline.default_instrumentation_service = service
pipeline = HTML::Pipeline.new [MarkdownFilter], context
pipeline.setup_instrumentation "MarkdownPipeline"

service.subscribe "call_filter.html_pipeline" do |event, start, ending, transaction |
  payload[:pipeline] # =>
  payload[:filter]   # =>
  payload[:doc]      # =>
  payload[:context]  # =>
  payload[:result]   # =>
  payload[:result][:output] # => 
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
###### simple form, wrappers api
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

```config/initializers/simple_form.rb
Dir[Rails.root.join('lib/components/**/*.rb')].each { |f| require f }
```

```lib/components/numbers_component.rb
module NumbersComponent
  def number(wrapper_options = nil)
    @number ||= begin
      options[:number].to_s.html_safe if options[:number].present?
    end
  end
end

SimpleForm.include_component(NumberComponent)
```

```.rb
class User
  include ActiveModel::Model
  
  attr_accessor :id, :name
end

```

