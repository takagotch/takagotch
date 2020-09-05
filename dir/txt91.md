###### reCAPTCHA
---


```recaptcha.html
<html>
<head>
<title></title>
<script src='https://www.google.com/recaptcha/api.js'></script>
</head>
<body>
<div class="g-recaptcha" data-sitekey="MYSITEKEY"></div>
</body>
</html>
```

```application.html.erb
<script src="https://www.google.com/recaptcha/api.js?render=reCAPCHASITEKEY"></script>
<script>
  grecaptcha.ready(function () {
    grecaptcha.execute('reCAPCHASITEKEY', { action: 'contact' }).then(function (token) {
      var recaptchaResponse = document.getElementById('recaptchaResponse');
      recaptchaResponse.value = token;
    });
  });
</script>
```

```sessions/new.html.erb
<% provide(:title, "Log in") %>
<h1></h1>

<div class="row">
  <div class="col-md-6 col-md-offset-3">
    <%= form_for(:session, url: login_path) do |f| %>
      <%= f.label :email %>
      <%= f.email_field :email, class: 'form-control' %>
      
      <%= f.label :password %>
      <%= link_to "(forgot password)", new_password_reset_path %>
      <%= f.password_field :password, class: 'form-control' %>
      
      <%= f.label :remember_me, class: "checkbox inline" do %>
        <%= f.check_box :remember_me %>
        <span>Remember me on this computer</span>
      <% end %>
      <input type="hidden" name="recaptcha_response" id="recaptcharResponse">
      
      <%= f.submit "Log in", class: "btn btn-primary" %>
    <% end %>
    
    <p>New user? <%= link_to "Sign up now!", signup_path %></p>
  </div>
</div>
```

```SessionsController/sessions_controller.rb
require 'uri'
require 'net/http'

class SessionController < ApplicationController

  RECAPTCHA_SECRET_KEY = 'reCAPTCHA CONSOLE SECRETKEY INPUT'
  RECAPTCHA_SITEVERIFY_URL = ''
  
  def create
    siteverify_uri = URI.parse()
    response = Net::HTTP.get_response(siteverify_uri)
    json_response = JSON.parse(response.body)
    
    if json_response['success'] && json_response['score'] > 0.5
      user = User.find_by(email: params[:session][:email])
      if
      
      
      else
      
      
      end
    else
      flash.now[:danger] = 'Invalid email/password combination'
      render 'new'
    end
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

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

