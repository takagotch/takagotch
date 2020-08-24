######
---

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


```
```

```
```


