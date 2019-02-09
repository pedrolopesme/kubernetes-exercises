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

#### Dashboard

Install Dashboard:
```
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
```

Access Dashboard at:
```
$ kubectl proxy
```

List Dashboard PODs:
```
kubectl get secret,sa,role,rolebinding,services,deployments --namespace=kube-system | grep dashboard
```

Remove all Dashboard PODS at once:
```
kubectl delete deployment kubernetes-dashboard --namespace=kube-system 
kubectl delete service kubernetes-dashboard  --namespace=kube-system 
kubectl delete role kubernetes-dashboard-minimal --namespace=kube-system 
kubectl delete rolebinding kubernetes-dashboard-minimal --namespace=kube-system
kubectl delete sa kubernetes-dashboard --namespace=kube-system 
kubectl delete secret kubernetes-dashboard-certs --namespace=kube-system
kubectl delete secret kubernetes-dashboard-key-holder --namespace=kube-system
```

**To Authenticate Using Token**

Create a new ServiceAccount
```
kubectl create serviceaccount k8sadmin -n kube-system
```

Create a ClusterRoleBinding with Cluster Admin Privileges
```
kubectl create clusterrolebinding k8sadmin --clusterrole=cluster-admin --serviceaccount=kube-system:k8sadmin
```

Then you should Generate a token:
```
$ kubectl get secret -n kube-system | grep k8sadmin | cut -d " " -f1 | xargs -n 1 | xargs kubectl get secret  -o 'jsonpath={.data.token}' -n kube-system | base64 --decode
```

Now you can access it via http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/ 
, then select authenticate via TOKEN and paste the generated Token in the previous step.


