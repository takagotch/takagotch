###### guard
---



```sh
gem install guard --no-ri --no-doc
gem install guard-rspec --no-ri --no-doc
guard init rspec

guard start

piccolo e 
```

```Guardfile
guard :rspec do
  watch(%r{^spec/.+_spec\.rb$})
  watch(%r{^lib/(.+)\.rb$})    { |m| "spec/lib/#{m[1]}_spec.rb" }
  watch('spec/spec_helper.rb') { "spec" }
  
  watch(%r{^app/(.+)\.rb$})                   { |m| "spec/#{m[1]}_spec.rb" }
  watch(%r{^app/(.*)(\.erb|\.haml|\.slim)$})  { |m| "spec/#{m[1]}#{m[2]}_spec.rb" }
  watch(%r{^app/controllers/(.+)_(controller)\.rb$}) { "spec" }
  watch(%r{^spec/support/(.+)\.rb$}) { "spec/routing" }
  watch('config/routes.rb') { "spec/routing" }
  watch('app/controllers/application_controller.rb') { "spec/controllers" }
  
  watch(%r{^app/views/(.+)/.*\.(erb|haml|slim)$}) { |m| "spec/features/#{m[1]}_spec.rb"}
  
  watch(%r{^app/views/(.+)/.*\.(erb|haml|slim)$})
  watch(%r{^spec/acceptance/steps/(.+)_steps\.rb$}) { |m| Dir[File.join("**/#{m[1]}.feature")][0] || 'spec/acceptance' }
end

guard :rspec do
  watch(%r{^spec/.+_spec\.rb$})
  watch(%r{^lib/(.+)\.rb$}) { |m| "spec/#{m[1]}_spec.rb" }
  watch('spec/spec_helper.rb') { "spec" }
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


