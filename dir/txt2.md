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

###### Jenkins

```
rake plugin
ruby plugin
ruby metrics plugin
```

```
```

```
```

```
```

```
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



