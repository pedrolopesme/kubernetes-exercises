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
### Service NodePort:

Create from `yaml`

```
$ kubectl create -f service-nodeport-definition.yaml
```

List services
```
$ kubectl get services
```

### Service Cluster IP:

Create from `yaml`

```
$ kubectl create -f service-clusterip-definition.yaml
```

List services
```
$ kubectl get services
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

### [cron]job-definition.yml 

Create from `yaml`
```
$ kubectl create -f [cron]job-definition.yml
```

Update POD using `yaml`:
```
# kubectl apply -f [cron]job-definition.yml
```



### rc-definition.yml:
Create from `yaml`
```
$ kubectl create -f rc-definition.yml
```

Update replication controller using `yaml`:
```
# kubectl replace -f rc-definition.yml
```

List replication controllers
```
$ kubectl get replicationcontroller
```

### replicaset-definition.yml:
Create from `yaml`
```
$ kubectl create -f replicaset-definition.yml
```

Update replica set using `yaml`:
```
# kubectl replace -f replicaset-definition.yml
```

Delete:
```
# kubectl delete replicaset myapp-replicaset
```

Edit:
```
# kubectl edit replicaset myapp-replicaset
```

List replica set
```
$ kubectl get replicaset
```

To scale:
```
$ kubectl scale --replicas=6 -f replicaset-definition.yml
```

or using replicaset name:
$ kubectl scale --replicas=6 replicaset myapp-replicaset

### deployment-definition.yml:
Apply from `yaml`
```
$ kubectl apply -f deployment-definition.yml
```

### namespace-definition.yml:
Create `dev` from `yaml`
```
$ kubectl apply -f namespace-definition.yml
```

Create `dev` from kubectl:
```
$ kubectl create namespace dev
```

Get resources from a specific namespace:
```
$ kubectl get pods --namespace=dev
```

Switching permanently to another namespace:
```
$ kubectl config set-context $(kubectl config current-context) --namespace=dev
```

Get resources from all namespaces:
```
$ kubectl get pods --all-namespaces
```

### resource-quota.yml:
Create from `yaml`
```
$ kubectl apply -f resource-quota.yml
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



### Others

#### To set a specific image:

```
$ kubectl set image deployment/my-deployment some-image=new-image:1.2 
```

#### Deployment status:

```
$ kubectl rollout status deployment/myapp-deployment
```

#### Deployment History

```
$ kubectl rollout history deployment/myapp-deployment
```

#### Deployment Undo

```
$ kubectl rollout undo deployment/myapp-deployment
```


#### Discovering deployment type:

```
$ kubectl describe deployment
```

#### Deployment types:

* recreate: terminate the old version and release the new one
* ramped: release a new version on a rolling update fashion, one after the other
* blue/green: release a new version alongside the old version then switch traffic
* canary: release a new version to a subset of users, then proceed to a full rollout
* a/b testing: release a new version to a subset of users in a precise way (HTTP headers, cookie, weight, etc.). A/B testing is really a technique for making business decisions based on statistics but we will briefly describe the process. This doesn’t come out of the box with Kubernetes, it implies extra work to setup a more advanced infrastructure (Istio, Linkerd, Traefik, custom nginx/haproxy, etc).

#### Restart Policy

Possible values Always, OnFailure, and Never. The default value is Always.


### Ingress 

#### Controller

Using NGINX as ingress controller:
* Deployment: nginx-ingress-controller.yaml
* Service: nginx-nodeport-service.yaml
* ConfigMap: nginx-configuration.yaml
* Auth: nginx-serviceaccount.yaml

#### Resource

Example for a demo app called `myapp` : ingress-sample-resource-to-myapp.yaml. Another exeample using paths is located at ingress-sample-resource-to-myapp-with-rules.yaml.

To get all ingress services:
```
$ kubectl describe ingress ingress-myapp
```
