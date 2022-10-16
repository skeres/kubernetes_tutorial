# goals : work with deployments
- run your first deployment


**Prerequisites :**
- A dockerhub account
- Docker installed on your machine for linux, Docker Desktop for Windows users
- Minikuke installed for Linux, or Docker Desktop with windows to run a Kubernetes cluster
- Kubectl installed ( comes without installation with Docker Desktop )
      
**TL;DR**  

## > run your first deployment 

### start Kubernetes cluster 
`minikube start`

`minikube addons enable dashboard`

`minikube addons enable metrics-server`

### in a new terminal, run dashboard in your favorite browser 
`minikube dashboard`

### create your yml file ( see directory resources/12 to get the file )
`vim mydeployment.yml`

### create the pod
`kubectl apply -f httpd_deployment.yml`

### verify that th pod is currently running. ( Type ctrl+c to exit when status is ready 1/1 )
`kubectl get pods --watch`

### display current pods
`kubectl get pods`  
**result :**  
*NAME                            READY   STATUS    RESTARTS   AGE*  
*myapp-deploy-589d4c5594-b4clj   1/1     Running   0          2m6s*  
*myapp-deploy-589d4c5594-cwpdh   1/1     Running   0          19m*  
*myapp-deploy-589d4c5594-f4gbh   1/1     Running   0          19m*  
*myapp-deploy-589d4c5594-gh5td   1/1     Running   0          19m*  

### see the deployment result in dashboard
it works !

![13_httpd_dashboard_deployment.png ](/resources/13_httpd_dashboard_deployment.png "13_httpd_dashboard_deployment")


### run port forwarding command to see the httpd server running
`kubectl port-forward deployments/myapp-deploy 9090:80`


### check your browser at localhost:9090
it works !

![13_httpd_running.png ](/resources/13_httpd_running.png "13_httpd_running")


### delete your deployment to end the game ;o)
`kubectl delete deployments.apps myapp-deploy `  

### Before closing your computer : stop minikube
`minikube stop`
 
Enjoy !! :sunglasses: :tropical_drink: :tropical_drink:

