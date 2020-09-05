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
    
    <p>New user? <%= link_to "Sign up now!"%></p>
  </div>
</div>
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

