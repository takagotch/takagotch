###### simplecov, simpleconv-rcov
---


```.sh
rails new jenkins_test --skip-bundle
cd jenkins_test

bundle install --path vendor/bundle
rails g rspec:install

rails g scaffold Article title:string
rake db:migrate

rake spec

```

```gemfile
group :development, :test do
  gem 'rspec'
  gem 'rspec-rails'
  gem 'simpleconv', :require => false
  gem 'simplecov-rcov', :require => false
end

```

```spec/spec_helper.rb
require 'simplecov'
require 'simplecov-rcov'
SimpleCov.formatter = SimpleCov::Formatter::RcovFormatter
SimpleCov.start 'rails'
```

###### Jenkins, CircleCI

```
rake plugin
ruby plugin
ruby metrics plugin

gem ''
gem ''
gem ''
```

```.sh
open coverage/index.html

echo "coverage" >> .gitignore
```

```.yml
- store_artifacts:
  path: coverage
```

```Gemfile
group :test do
  gem 'simplecov'
end

```

```spec/spec_helper.rb
require 'simplecov'

SimpleCov.minimum_coverage 90

SimpleCov.minimum_coverage_by_file 80

SimpleCov.start do
  add_filter '/spec/'
  add_filter do |source_file|
    source_file.lines.count < 5
  end
  
  enable_coverage :branch
  
  add_group 'Models', 'app/modles'
  add_group 'Services', 'app/services'
  add_group 'Controllers (api)', 'app/controllers/api'
  add_group 'Controllers', 'app/controllers'
  add_group 'Helpers', 'app/helpers'
end

```

```coverage/index.html
```

```app/models/user.rb
```

```spec/models_user_spec.rb
# frozen_string_literal: true

require 'rails_helpers'

describe Foo, type: :model do
  it do
    
  end
end

# :nocov:
def skip_this_method
  never_reached
end

```

###### selenium, for browser, capybara

```
gem install rspec
gem install selenium-webdriver

rspec selenium_test.rb
```

```tests/selenium_test.rb
#encoding: utf-8
require "selenium-webdriver"
require "rspec"
include RSpec::Expectations

describe "SeleniumTest" do
  before(:each) do
    @driver = Selenium::WebDriver.for :ie
    @base_url = "http://xxx.xxx.xxx.xxx/"
    @driver.manage.timeouts.implicit_wait = 30
    @verifycation_errors = []
  
end

```

```
```

```rails_helper.rb
if ENV['COVERAGE']
  require 'simplecov'
  require 'simplecov-rcov'
  SimpleCov.start 'rails' do
    add_filter '/vendor'
  end
  SimpleCov.formatter = SimpleCov::Formatter::RcovFormatter
end
```

```
```

```
```

###### brakeman

```.sh
brakeman
brakeman --only-files=path/to/specific_file
```

```
```

###### bullet

```config/environments/development.rb
User::Application.configure do
  config.after_initialize do
    Bullet.enable = true
    Bullet.bullet_logger = true
    Bullet.stacktrace_includes
  end
end

```

```
```

###### pre-commit

```.sh
pre-commit install
pre-commit disable yaml checks rubocop
```

```
```

###### rubocop

```.sh
rubocop --auto-gen-config
rubocop -a path/to/file.rb
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```



