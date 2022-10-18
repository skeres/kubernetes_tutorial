# Goals : work with namespaces, labels and selectors
- create namespace and limit it with quotas
- add labels to kubernetes objects and requests list objects with selectors

**Inspire from tutorial**  
[Kubernetes namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)  
[Kubernetes labels and selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)  

**useful links**
[none](https://www.google.fr)  

**Prerequisites :**
- A dockerhub account
- Docker installed on your machine for linux, Docker Desktop for Windows users
- Minikuke installed for Linux, or Docker Desktop with windows to run a Kubernetes cluster
- Kubectl installed ( comes without installation with Docker Desktop )
      
**TL;DR**  
## > Context
Play with Kubernetes

### start Kubernetes cluster 
`minikube start`

`minikube addons enable dashboard`

`minikube addons enable metrics-server`


## >> Create namespaces and limit resources with quotas

### List all existing namespaces
`kubectl get ns`
**result**  
*NAME                   STATUS   AGE*  
*default                Active   130d*  
*ingress-nginx          Active   27h*  
*kube-node-lease        Active   130d*  
*kube-public            Active   130d*  
*kube-system            Active   130d*  
*kubernetes-dashboard   Active   127d*  


### Create your own namespace
tip : namespace in kubernetes exist to avoid colisions between to object having the same name.
`kubectl create namespace my-namespace-01`
**result**  
*namespace/my-namespace-01 created*  

### Check the content of your new namespace
`kubectl get pods --namespace=my-namespace-01`  
or  
`kubectl get pods -n=my-namespace-01`  
**result**  
*No resources found in my-namespace01 namespace.*  

### Create a pod running in your namespace
tip : you should specify a namespace, because if you don't, kubectl will create your new object (pod, job ...) in "default" namespace
`kubectl run first-pod --image=nginx --restart=Never --namespace=my-namespace-01`  
**result**  
*pod/first-pod created*  

### Check pods of your namespace
`kubectl get pods -n=my-namespace-01`  
**result**  
*NAME        READY   STATUS    RESTARTS   AGE*  
*first-pod   1/1     Running   0          2m41s*  

### You can also create a namespace with a manifest file
create your yml file ( see directory resources/10 to get the file )  
`vim createNameSpace.yml`  

apiVersion: v1  
kind: Namespace  
metadata:  
  name: my-namespace-02  


### Create the new namespace my-namespace-02
create your yml file ( see directory resources/10 to get the file )  
`kubectl apply -f createNameSpace.yml`  
**result**  
*namespace/my-namespace-02 created*  


### List all existing namespaces
`kubectl get ns`
**result**  
*NAME                   STATUS   AGE*  
*default                Active   130d*  
*ingress-nginx          Active   27h*  
*kube-node-lease        Active   130d*  
*kube-public            Active   130d*  
*kube-system            Active   130d*  
*kubernetes-dashboard   Active   127d*  
*my-namespace-01        Active   19m*  
*my-namespace-02        Active   5m12s*  

### Add quota to my-namespace-01
tip : you can limit the number of objects in a namespace  
1/ create your yml file ( see directory resources/10 to get the file )  
`vim quota-resources.yml`  

apiVersion: v1  
kind: ResourceQuota  
metadata:  
  name: podquota  
spec:  
  hard:  
    pods: 10  
    
2/ apply quota to my-namespace-01
`kubectl create -f quota-resources.yml -n=my-namespace-01`  
**result**  
*resourcequota/podquota created*  


### display resources for my-namespace-01
`kubectl describe resourcequotas -n=my-namespace-01`  
**result**  
*Name:       podquota*  
*Namespace:  my-namespace-01*  
*Resource    Used  Hard*  
*--------    ----  ----*  
*pods        1     10*  


## >> add labels and requests with selectors
tip : you can add labels to kubernetes objects to select them quicker  
Labels are just your own key/value you add to objects  

### show all labels on all existing namespaces
`kubectl get namespaces --show-labels`  
**result**   
*NAME                   STATUS   AGE    LABELS*  
*default                Active   130d   kubernetes.io/metadata.name=default*  
*kube-node-lease        Active   130d   kubernetes.io/metadata.name=kube-node-lease*  
*kube-public            Active   130d   kubernetes.io/metadata.name=kube-public*  
*kube-system            Active   130d   kubernetes.io/metadata.name=kube-system*  
*my-namespace-01        Active   35m    kubernetes.io/metadata.name=my-namespace-01*  
*my-namespace-02        Active   21m    kubernetes.io/metadata.name=my-namespace-02*  


### add one label "type=with-limit" ( or more ;o) ) on namespace my-namespace-01
`kubectl label namespaces my-namespace-01 type=with-limit`  
**result**   
*namespace/my-namespace-01 labeled*  


### now you can easily retrieve namespaces having a label like "type" value "with-limit"
`kubectl get namespaces --selector type=with-limit`  
or
`kubectl get namespaces -l type=with-limit`
**result**   
*NAME              STATUS   AGE*  
*my-namespace-01   Active   45m*  

tip : you can also add a column in the result to see the value of the key ofr each object  
Example : to list object with their "type" value label  
`kubectl get namespaces -Ltype`
**result**   
*NAME                   STATUS   AGE    TYPE*  
*default                Active   130d*   
*kube-node-lease        Active   130d*   
*kube-public            Active   130d*   
*kube-system            Active   130d*   
*my-namespace-01        Active   50m    with-limit*  
*my-namespace-02        Active   36m*    



### Delete resources of this game
`kubectl delete pod -n=my-namespace-01 first-pod`  
`kubectl delete namespaces my-namespace-01`  
`kubectl delete namespaces my-namespace-02`  

### Before closing your computer : stop minikube
`minikube stop`  
 
Enjoy !! :sunglasses: :tropical_drink: :tropical_drink:

