###### Fast_jsonapi



###### blanket
---



```.rb
require 'blanket'

github = Blanket.wrap("https://api.github.com")

user = github.users('inf0rmer').get
user.login

github.users('inf0rmer').repos.get
# => [{
#   "id": 20000073,
#   "name": "xxx",
# }]

github = Blanket.wrap("https://api.github.com")
github.users('inf0rmer').repos.git

github = Blanket.wrap("https://api.github.com")
# => "https://api.github.com"

github.users
# => "https://api.github.com/users"

github.users('inf0rmer')
# => "https://api.github.com/users/inf0rmer"

github.users('inf0rmer').repos
# => "https://api.github.com/users/inf0rmer/repos"
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

```
```

