# kubernetes-exercises
Forget it. Its not for you. Just some kubernetes configuration exercises.

### general cmds

List PODs:
```
$ kubectl get pods
```

Show POD info:
```
$ kubectl describe pod <POD NAME>
```

Stop a POD:
```
$ kubectl stop pod nginx-7cdbd8cdc9-wzht9
```

Run a POD from a docker image
```
$ kubectl run nginx --image nginx
```


### pod-configuration.yml:

Create from `yaml`
```
$ kubectl create -f pod-definition.yml
```

Update POD using `yaml`:
```
# kubectl apply -f pod-definition.yml
```

### Others

Install Dashboard:
```
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
```



