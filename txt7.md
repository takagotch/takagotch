###### kubernates-helm | deploy
###### docker-compose -f docker-compose.yml -f production.yml up -d | deploy
---


```sh
docker-compose -f docker-compose.yml -f production.yml up -d 
docker-compose build web
docker-compose up --no-deps -d web




// linux
mv linux-amd64/helm /usr/local/bin/helm
helm init
helm init --upgrade

```

```sh
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
helm -h
helm search repo
helm repo update
helm install rails stable/rails
helm list
kubectl get po
kubectl get deploy
kubectl get statefulset
kubectl get cm
kubectl get svc
helm upgrade rails stable/rails --set railsBlogName=blog1
helm history rails
helm rollback rails 1
helm uninstall rails
helm pull stable/rails
ls rals-6.0.3.2.tgz
tar xvzf rails-6.0.3.2.tgz
ls rails
helm install rails ./rails

ls charts
helm dependency update
helm test rails
helm create ex1
ls -R ex1/
```

```sh
```

```sh
```

```sh
```

```sh
```

```sh
```

```sh
```

