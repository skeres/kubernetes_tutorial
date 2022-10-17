# Goals : work with Ingress Controller for K8s dashboard
- run your first Ingress Controller


**Prerequisites :**
- A dockerhub account
- Docker installed on your machine for linux, Docker Desktop for Windows users
- Minikuke installed for Linux, or Docker Desktop with windows to run a Kubernetes cluster
- Kubectl installed ( comes without installation with Docker Desktop )
      
**TL;DR**  
## > Context
The K8s dashoard is not accesible from outside the cluster. We're going to implemente an Ingress controller  
to permit access from your favorite browser using some domain name.  

## >> Run your first Ingress Controller

### Install Ingress Controller in Minikube
out of the box, it will install and automatically start the K8s Nginx implementation of Ingress Controller,  
one of the existing third party implementions.  
`minikube addons enable ingress`

### Display existing namespaces
`kubectl get ns`  
or  
`kubectl get --namespace`  
**result**  
*a@a-w541:~/projets/projets_kubernetes/kubernetes_tutorial$ kubectl get namespaces*  
*NAME                   STATUS   AGE*  
*default                Active   129d*  
*ingress-nginx          Active   10m*  
*kube-node-lease        Active   129d*  
*kube-public            Active   129d*  
*kube-system            Active   129d*  
*kubernetes-dashboard   Active   126d*  

you can also display all running pods from all namespaces  
`kubectl get pods --all-namespaces`  

### Display all the component you have for the dashboard
`kubectl get all -n kubernetes-dashboard`  
![21_get_all_dashboard.png ](/resources/21_get_all_dashboard.png "21_get_all_dashboard")

### Create your yml file ( see directory resources/21 to get the file )
Those rules will forward all requests from mydashboard.com to the *internal* kubernetes Service type "ClusterIP" named kubernetes-dashboard.  

### Create the Ingress rules for th dashboard
`kubectl apply -f dashboard-ingress.yml`  
**result**  
*ingress.networking.k8s.io/dashboard-ingress created*  

### Verify that Ingress is currently running. ( Type ctrl+c to exit when status is ready : ip adress is displayed)
`kubectl get ingress -n kubernetes-dashboard --watch`  
**result :**  
*NAME                CLASS   HOSTS             ADDRESS        PORTS   AGE*  
*dashboard-ingress   nginx   mydashboard.com   192.168.49.2   80      3m23s*  

### You can also display informations about your new rules for your nginx Ingress in the namespace kubernetes-dashboard
`kubectl describe ingress dashboard-ingress -n kubernetes-dashboard`  
![21_describe_ingress.png ](/resources/21_describe_ingress.png "21_describe_ingress")  

### To simulate a DNS resolution, define a mapping between your Ingress IP (here 192.168.49.2) and mydashboard.com
**care ! always make a backup of systems files before update, so you will be able to rollback if any problem appears**  
`sudo cp /etc/hosts /etc/hosts_backup_today`  
backup is done !  

`sudo vim /etc/hosts`  
 add the new mapping to route incoming requests to the dashboard  
![21_vim_etc_hosts.png ](/resources/21_vim_etc_hosts.png "21_vim_etc_hosts")  

### Access to your dasboard in your favorite browser
`http://mydashbard.com`  

it works !   
![21_mydashboard.com.png ](/resources/21_mydashboard.com.png "21_mydashboard.com")  




### Delete your Ingress and rollback to your first /etc/hosts to end the game ;o)
`kubectl delete ingress dashboard-ingress -n kubernetes-dashboard`  
`sudo cp /etc/hosts_backup_today /etc/hosts`  


### Before closing your computer : stop minikube
`minikube stop`  
 
Enjoy !! :sunglasses: :tropical_drink: :tropical_drink:

