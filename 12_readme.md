# Goals : DECLARATIVE mode with ngninx, dashboard and replicaset
- deploy your first Nginx Pod 
- install kubernetes dashboard
- deploy your first replicaset


**Prerequisites :**
- A dockerhub account
- Docker installed on your machine for linux, Docker Desktop for Windows users
- Minikuke installed for Linux, or Docker Desktop with windows to run a Kubernetes cluster
- Kubectl installed ( comes without installation with Docker Desktop )
      
**TL;DR**  

## > Deploy your first Nginx Pod 

### Start Kubernetes cluster 
`minikube start`

### Create your yml file ( see directory resources/12 to get the file )
`vim nginxpod.yml`

### Create the pod
`kubectl apply -f nginxpod.yml`

### Verify that th pod is currently running. ( Type ctrl+c to exit when status is ready 1/1 )
`kubectl get pods --watch`



## > Install kubernetes dashboard   

### Minimum installation 
`minikube addons enable dashboard`

### Add metrics 
`minikube addons enable metrics-server`

### Run dashboard in your favorite browser 
`minikube dashboard`

it works !

![12_nginx_dashboard.png ](/resources/12_nginx_dashboard.png "Your nginx server informations in kubernetes dashboard")

### Delete pod using dashboard
![12_nginx_dashboard_delete.png ](/resources/12_nginx_dashboard_delete.png "delete pod")


## > Deploy your first 
   

### create your yml file ( see directory resources/12 to get the file )
`vim nginx_replicaset.yml`

### create the pod
`kubectl apply -f nginx_replicaset.yml`

it works !

![12_nginx_dashboard_replicaset.png](/resources/12_nginx_dashboard_replicaset.png "Your replicaset")


### Display list of replicasets
`kubectl get rs`  
**result :**  
*NAME            DESIRED   CURRENT   READY   AGE*  
*my-replicaset   4         4         4       6m11s*  

### Display current pods
`kubectl get pods`  
**result :**  
*NAME                  READY   STATUS    RESTARTS   AGE*  
*my-replicaset-cjpsc   1/1     Running   0          9m7s*  
*my-replicaset-dt8pt   1/1     Running   0          9m7s*  
*my-replicaset-gjvkd   1/1     Running   0          9m7s*  
*my-replicaset-l2rb9   1/1     Running   0          9m7s*  

please note the structure of pod's names : kubernetes add some letters and numbers !


### Display informations about replicaset
`kubectl describe rs my-replicaset`  
**result :**  
![12_nginx_describe_replicaset.png](/resources/12_nginx_describe_replicaset.png "Your replicaset")


### Try to delete a pod => kubernetes will re-create it !
`kubectl delete pod my-replicaset-dt8pt`  

### Verify that a new pod has been created
`kubectl get pods`     
*NAME                  READY   STATUS    RESTARTS   AGE*  
*my-replicaset-cjpsc   1/1     Running   0          24m*  
*my-replicaset-gjvkd   1/1     Running   0          24m*  
*my-replicaset-h99rq   1/1     Running   0          71s*  >>> h99rq is the new pod. dt8pt above has been removed  
*my-replicaset-l2rb9   1/1     Running   0          24m*   

### You can scale up or down the replicaset size ! 
`kubectl scale --replicas=2 replicaset my-replicaset`  
**result :**  
![12_nginx_scale_down_replicaset.png](/resources/12_nginx_scale_down_replicaset.png "Your replicaset")


### Delete your replicaset to end this game ;o)
`kubectl delete rs my-replicaset`  

### Before closing your computer : stop minikube
`minikube stop`
 
Enjoy !! :sunglasses: :tropical_drink: :tropical_drink:

