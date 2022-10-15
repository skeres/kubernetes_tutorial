# goals : with minikube cluster, using DECLARATIVE mode : 
# >>> deploy your first Nginx Pod 
# >>> install kubernetes dashboard
# >>> deploy your first replicaset


**Prerequisites :**
- A dockerhub account
- Docker installed on your machine for linux, Docker Desktop for Windows users
- Minikuke installed for Linux, or Docker Desktop with windows to run a Kubernetes cluster
- Kubectl installed ( comes without installation with Docker Desktop )
      
**TL;DR**  

## >>> Deploy your first Nginx Pod 

### start Kubernetes cluster 
`minikube start`

### create your yml file ( see directory resources/12 to get the file )
`vim nginxpod.yml`

### create the pod
`kubectl apply -f nginxpod.yml`

### verify that th pod is currently running. ( Type ctrl+c to exit when status is ready 1/1 )
`kubectl get pods --watch`



## >>> Install kubernetes dashboard   

### minimum installation 
`minikube addons enable dashboard`

### add metrics 
`minikube addons enable metrics-server`

### run dashboard in your favorite browser 
`minikube dashboard`

it works !

![12_nginx_dashboard.png ](/resources/12_nginx_dashboard.png "Your nginx server informations in kubernetes dashboard")









----------------------------------------------------------------
### check logs of a spécific pod 
`kubectl logs premier-pod`

### forwarding ports from your cluster 80 to your computer 8080
`kubectl port-forward pods/premier-pod 8080:80`

Visit http://127.0.0.1:8080 on your favorite browser

![nginx_it_works](/resources/nginx_it_works.png "Your nginx server works at http://localhost:8080/")

### get a bash session in your pod, exit from pod, and delete it from your console to terminate TP
open a new terminal/console

`kubectl exec -it premier-pod -- bash`  
**result :**  
*root@premier-pod:/#*  

`hostname -i`  
**result :**  
*172.17.0.5*  
*root@premier-pod:/#*  

`exit`  
**result :**  
*a@a-w541:~$*  

`kubectl delete pod premier-pod`  
**result :**  
*pod "premier-pod" deleted*  
*a@a-w541:~$*  

`kubectl get pods  
**result :**  
*No resources found in default namespace.*  
*a@a-w541:~$*  



### Before closing your computer : stop minikube
`minikube stop`
 
  
 
 
Enjoy !! :sunglasses: :tropical_drink: :tropical_drink:
