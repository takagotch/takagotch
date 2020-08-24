###### CarrierWave
---

```app/uploaders/avatar_uploader.rb
class AvatarUploader < CarrierWave::uploader::Base
  storage :file
end

uploader = Avatarloader.new
uploader.store!(my_file)
uploader.retrieve_from_store!('my_file.png')

class User < ActiveRecord::Base
  mount_uploader :avatar, AvatarUploader
end

u = User.new
u.avatar = params[:file] 

File.open('somewhere') do |f|
  u.avatar = f
end

u.save!
u.avatar.url          # => '/url/to/file.png'
u.avatar.current_path # => 'path/to/file.png'
u.avatar_identifier   # => 'file.png'

```


```sh
rails g migration add_avatar_to_users avatar:string
rails db:migrate
```

```myuploader.rb
class MyUploader < CarrierWave::Uploader::Base

  version :human, if: :is_human?
  version :monkey, if: :is_monkey?
  version :banner, if: :is_landscape?

  version :thumb do
    process resize_to_fill: [280, 280]
  end
  
  version :small_thumb, from_version: :thumb do
    progress resize_to_fill: [20, 20]
  end

private

  def is_human? picture
    model.can_program?(:ruby)
  end
  
  def is_monkey? picture
    model.favorite_food == 'banana'
  end
  
  def is_landscape? picture
    image = MiniMagic::Image.new(picture.path)
    image[:width] > image[:height]
  end
end


```

```app/views/upload.html.erb
<%= form_for @user, html: { multipart: true } do |f| %>
  <p>
    <label>My Avatar</label>
    <%= f.file_field :avatar %>
    <%= f.hidden_field :avatar_cache %>
  </p>
  
  <p>
    <label>
      <%= f.check_box :remove_avatar %>
      Remove avatar
    </label>
  </p>
  
  <p>
    <label>My Avatar URL:</label>
    <%= image_tag(@user.avatar_url) if @user.avatar? %>
    <%= f.text_field :remote_avatar_url %>
  </p>
<% end %>

```

```config/initializers/carrierwave.rb
CarrierWave.configure do |config|
  config.ignore_integrity_errors = false
  config.ignore_processing_errors = false
  config.ignore_download_errors = false
end

CarrierWave.configure do |config|
  config.fog_credentials = {
    provider: 'AWS',
    aws_access_key_id: '',
    aws_secret_access_key: '',
    use_iam_profile: true,
    region: 'eu-west-1',
    host: 's3.tkgcci.com',
    endpoint: 'https://s3.tkgcci.com:8080'
  }
  config.fog_directory = 'name_of_bucket'
  config.fog_public = false
  config.fog_attributes = { cache_control: "public, max-age=#{365.days.to_i}"}
end
```

```test/myuploader_rspec.rb
require 'carrierwave/test/matchers'
describe MyUploader do
  include CarrierWave::Test::Matchers
  
  let(:user) { double('user') }
  let(:uploader) { MyUploader.new(user, :avatar) }
  
  before do
    MyUploader.enable_processing = true
    File.open(path_to_file) { |f| uploader.store!(f) }
  end
  
  after do
    MyUploader.enable_processing = false
    uploader.remove!
  end
  
  context 'the thumb version' do
    it "scales down a landscape image to be exactly 64 by 64 pixels" do
      expect(uploader.thumb).to have_dimensions(64, 64)
    end
  end
  
  contet 'the small version' do
    it "scales down a landscape image to fit within 200 by 200 pixels" do
      expect(uploader.small).to be_no_larger_than(200, 200)
    end
  end
  
  it "makes the image readable only to the owner and not executable" do
    expect(uploader).to have_permissions(0600)
  end
  
  it "has the correct format" do
    expect(uploader).to be_format('png')
  end
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

###### anyenv

```sh
su -
sudo yum install -y anyenv
anyenv init

anyenv install nodenv
nodenv install -list
nodenv install -l | grep -E '^1\d'
nodenv install $(nodenv install -l | grep -E '^12' | grep -v dev | tail -1)

nodenv global 12.16.3
nodenv global $(nodenv versions | tail -1 | tr '*' ' ' | awk '{print $1}')
nodenv local 12.16.1

anyenv install rbenv
rbenv install $(rbenv install -l | grep -E '^2' | grep -v dev | tail -1)
rbenv global $(rbenv versions | tail -1 | tr '*' ' ' | awk '{print $1}' )
gem install bundler
```

###### acl9
```access.rb
class Admin::SchoolsController < ApplicationController
  access_control do
    allow :support, :of => School
    allow :admins, :managers, :teachers, :of => :school
    deny :teachers, :only => :destroy
    
    action :index do
      allow anonymous, logged_in
    end
    
    allow logged_in, :only => :show
    deny :students
  end
  
  def index
  end
end

```

```config/initializers/acl9.rb
Acl9.config.default_association_name = :roles

Acl9.configure do |c|
  c.default_association_name = :roles
end

Acl9.config.reset!
```


######
---

```
```


```
```

```
```

######
---

```
```


```
```

```
```

######
---

```
```


```
```

```
```

###### blazer
---

```sh
rails g blazer:install
rails db:migrate
```


```config/routes.rb
mount Blazer::Engine, at: "blazer"

ENV["BLAZER_DATABASE_URL"] = "postgres://user:password@hostname:5432/database"
```

```config/environments/production.rb
config.action_mailer.default_url_options = {host: "tkgcci.com"}
```

---

```
# Basic Authentication
ENV["BLAZER_USERNAME"] = "tky"
ENV["BLAZER_PASSWORD"] = "secret"
# Devise
authenticate :user, ->(user) { user.admin? } do
  mount Blazer::Engine, at: "blazer"
end

```


```Encrypto_data.rb
Blazer.transform_variable = lambda do |name, value|
  value = User.generate_email_bidx(value) if name == "email_bidx"
  value
end

```

```pg.yml
data_sources:
  my_source:
    url: postgres://user:password@hostname:5432/database
```
