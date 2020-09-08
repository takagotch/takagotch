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
```

```
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

```
```

```
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

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```



