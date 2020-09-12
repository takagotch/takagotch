###### (mail)[https://github.com/mikel/mail]
---


```usage.rb
mail = Mail.new do
  from    'tky@gmail.com' 
  to      'info@gmail.com'
  subject 'This is a test email'
  body    File.read('body.txt')
end

mail.to_s # =>










```

```spec/usage.rb
Mail.defaults do
  delivery_method :test
end

describe "" do
  include Mail::Matchers
  
  before(:each) do
    Mail::TestMailer.deliveries.clear
    
    Mail.deliver do
      to      ['tky@gmail.com', 'tky2@gmail.com']
      from    'info@gmail.com'
      subject 'test'
      body    'text'
    end
  end
  
  it { is_expected.to have_sent_email }
  
  it { is_expected.to have_sent_email.from('tky@gmail.com') }
  it { is_expected.to have_sent_email.to('tky1@gmail.com') }
  
  it { is_expected.to have_sent_email.to(['tky@gmail.com', 'tky2@gmail.com']) }
  
  it {}
  
  it {}
  it {}
  
  it {}
  it {}
  
  
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

