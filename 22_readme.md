# Goals : execute jobs and scheduling
- run your job
- scheduling your job

**Inspired from tutorial**  
[jobs youtube video - starts at 3.55.05 ](https://www.freecodecamp.org/news/learn-docker-and-kubernetes-hands-on-course/)  

**Prerequisites :**
- A dockerhub account
- Docker installed on your machine for linux, Docker Desktop for Windows users
- Minikuke installed for Linux, or Docker Desktop with windows to run a Kubernetes cluster
- Kubectl installed ( comes without installation with Docker Desktop )
      
**TL;DR**  
## > Context
You can run specific task on demand with jobs.  
- A **Job** is a workload off short lived tasks
- Creates one or more Pods and ensures that a specified number of them successfully terminate.
- As Pod successfully complete, the **Job** tracks the successful completions.
- When a specified number of successful completions is reached, the **Job** completes.
- By default, **Jobs** with more than 1 Pod will create them one after the other. To create them at the same time, add parallelims.

## >> Run your first job

### Cheats for Jobs
|**COMMAND**   |**DESCRIPTION**   |
|---|---|
|kubectl apply -f \[jobname.yml\]     | - create Job |
|kubectl get job   | - list jobs |
|kubectl describe job \[jobname\]  | - display information |
|kubectl delete -f \[jobname.yml\]    | - delete the Job using file definition |
|kubectl delete job \[jobname\]    | - delete the Job using jobname |

### Create your own namespace
tip : namespace in kubernetes exist to avoid colisions between to object having the same name.  
`kubectl create namespace my-namespace-jobs`  


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
*my-namespace-jobs      Active   88s*  

you can also display all running pods from all namespaces  
`kubectl get pods --all-namespaces`  


### Create your yml file ( see directory resources/22 to get the file )
my_job_01.yml  
content :   
```
apiVersion: batch/v1
kind: Job
metadata:
  name: myjob  # this will be the job name
  labels:
    batch: myfirstbatch
    type: job
spec:
  activeDeadlineSeconds: 30
  parallelism : 1
  completions : 1
  template:
    spec:
      containers:
      - name: busybox-container # this is will be container's name
        images : busybox  
        command : ["echo", "Hello from the world !"]
      restartPolicy : Never
```


### Run your Job in the namespace you have created (otherwise, it will be "default" namespace)
`kubectl apply -f my_job_01.yml --namespace my-namespace-jobs`  

### Check Jobs
`kubectl get job --namespace my-namespace-jobs`   
**result**
*NAME    COMPLETIONS   DURATION   AGE*  
*myjob   1/1           2s         15m*  



## Play with Kubernetes Dashboard

Check Kubernetes Dashboard to see the results ( choose or type for namespace "my-namespace-jobs" )  
**result**  
it works !   
![22_job_succeed.png ](/resources/22_job_succeed.png "22_job_succeed")  


Check logs of the Job to see the "hello world" result. Access like this :   
![22_job_log_access.png ](/resources/22_job_log_access.png "22_job_log_access")  

it works !   
![22_job_result.png ](/resources/22_job_result.png "22_job_result")  


### Other way to see the log file
`kubectl get pods --namespace my-namespace-jobs`  
**result**  
*NAME          READY   STATUS      RESTARTS   AGE*  
*myjob-2bxz7   0/1     Completed   0          31m*  

and with the name of the Pod :   
`kubectl logs --namespace my-namespace-jobs myjob-2bxz7`  
**result**  
*Hello from the world !*  



### Clear your environment to end the game ;o)
`kubectl delete -f my_job_01.yml --namespace my-namespace-jobs`  
`kubectl delete namespace my-namespace-jobs`  



### Before closing your computer : stop minikube
`minikube stop`  
 
Enjoy !! :sunglasses: :tropical_drink: :tropical_drink:

