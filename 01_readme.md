# Goals : playing with Minikube
- prerequisites
- installing tools

**Resources**  
[Minikube home page  ](https://minikube.sigs.k8s.io/docs/)  
[Minikube installation ](https://minikube.sigs.k8s.io/docs/start/)  

**What will you need ?**
- A Dockerhub account. [Dockerhub home page ](https://hub.docker.com/)  
- Docker installed on your machine for linux, Docker Desktop for Windows users. For Ubuntu users, see [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)  
- Kubectl installed ( comes without installation with Docker Desktop on Windows ). For Ubuntu users, see [Install and Set Up kubectl on Linux](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)  
      
**TL;DR**  
## > Context
with Minikube, you will have your own Kubernetes cluster on your machine !  

## >> Start your first Cluster !

### Version of Kubectl and your Kubernetes Cluster
TODO  

### Install Minikube
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```
```
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

### Run the cluster
```
minikube start --cpus=4 --memory=4000 --kubernetes-version=v1.25.2
```
**result**
![01_start_minikube.png ](/resources/01_start_minikube.png "01_start_minikube")  


### Run your first application
TODO  




