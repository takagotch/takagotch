######
---

```
```


```
```

```
```

###### rack tracker, rack google analytics

```rack-tracker.rb
config = MyApplication::Application.config
config.middleware.use(Rack::Tracker) do
  handler :google_analytics, { tracker: 'U-xxxx-Y' }
end

tracker do |t|
  t.google_analytics :send, { type: 'event', category: 'CATEGORYNAME', action: 'ACTIONNAME', label: 'LABELNAME', value: 'VALUE' }
end

def show
  tracker do |t|
    t.google_analytics :send, { type: 'event', category: 'button', action: 'click', label: 'nav-buttons', value: 'X' }
  end
end

```

######
```
```

```
```


