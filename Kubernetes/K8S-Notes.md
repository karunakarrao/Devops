

Kubernetes: Overview
-----------------------------------------------------------------
Kubernetes is an **Open-Source** and container orchestration system for automate software deployment, scale, descale, auto-scale, deploy, replicate, loadbalance, failover. its originally developed by Google.  which is used widely in containerization platfroms. it has widely spread global community support. now it maintained by **Cloud Native Computing Foundation**.

k8s roles change according to your certification and responsibilities. 

	1. KCNA - Kubernetes and Cloud-Native Associate
	2. CKAD - Certified Kubernetes Application Developer
	3. CKA - Certified Kubernetes Administrator
	4. CKSA - Certified Kubernetes Security Specialist

there are other tools avaiable in the market, but k8s is the widly used one. other tools like

    1. OpenShift
    2. DockerSwarm 
    3. Azure AKS
    4. AWS EKS

Kubernetes installtion:
-----------------------------------------------------------------
k8s installtion can be done is different ways depeding on the requirement.

    1. minikube		--> for testing, training or local setup we can use it.
    2. kubeadm 		--> for commertial purpose setup, or production grade setup require kubeadm.
    3. kops 		--> A command-line utility specifically for creating, managing, and upgrading Kubernetes clusters on AWS.
    4. Kubespray  	--> A set of Ansible scripts used for deploying production-ready Kubernetes clusters.
    5. Rancher		--> An open-source platform. Rancher makes it easier to deploy and manage Kubernetes clusters across different environments.
    6. Kind (Kubernetes in Docker) --> Allows you to run a Kubernetes cluster on your local machine using Docker containers as nodes. It's primarily used for testing and development workflows.
    7. AWS - EKS 	--> Amazon Elastic Kubernetes Service (EKS) is a managed Kubernetes service provided by Amazon Web Services (AWS). 
    8. Google (GKE) 	--> Google Kubernetes Engine (GKE) is (GCP) managed Kubernetes service that simplifies the deployment, management, and scaling of containerized applications using Kubernetes.
    9. Azure (AKS) 	--> Azure Kubernetes Service (AKS) is Azure's managed k8s service offering deploying, managing, and scaling containerized applications using Kubernetes on the Azure cloud platform.

Kubernetes-architecture:
-----------------------------------------------------------------
Kuberenetes architecture build on master & salve model. In K8S there are several components involved. below are the list of components. 

    Master-Node components: Kuber API-Server, ETCD, kube-Controller, kube-Scheduler, Container-runtime, kubelet(agent), kubeproxy(agent)
    Worker-Node Components: Container-Runtime(Docker), kubelet(agent), kubeproxy(agent)
    

K8s -Install:
-----------------------------------------------------------------
All configurations of K8s are stored in the below loaction.

 	1. kubectl, kubeadm, kubelet, kube-proxy are available in (or) /usr/local/bin
	2. Configuration files for Kubernetes, such as kubeconfig files (which specify cluster details, authentication, and more), are commonly located in the user's home directory under ~/.kube/.
 	3. Kubernetes Configuration Directory: configuration files specific to Kubernetes are reside in /etc/kubernetes, /etc/kubernetes/manifests
  	4. Kubelet Data: Data associated with kubelet (like certificates, logs, and other runtime information) can be found in directories like (/var/lib/kubelet/).
   	5. If you're using container runtimes like Docker, data are stored in their respective directories (/var/lib/docker/).
    	6. Logs for various Kubernetes components, including the API server, controller-manager, scheduler, and worker nodes, might be stored in /var/log/.
     
       		1. API server logs: 		--> $ journalctl -u kube-apiserver
		2. Controller-manager logs: 	--> $ journalctl -u kube-controller-manager
		3. Scheduler logs: 		--> $ journalctl -u kube-scheduler

  	     	/etc/systemd/system/      	--> all service files are avaiable here
		/etc/kubernetes			--> k8s installtion directory
  		/etc/kubernetes/pki		--> k8s certificates store
  		/etc/kubernetes/manifest	--> K8s manifest files (etcd.yaml,  kube-apiserver.yaml,  kube-controller-manager.yaml,  kube-scheduler.yaml)
    		/var/lib/kubelet		--> k8s object are maintianed here like (pods, containers, logs, volumes)
      		/var/log/pods			--> pods log information is recorded. 

1). kube-API-server: 
-----------------------------------------------------------------
Kubernetes API-Server is the front-end for K8s, this will allow users interact with their Kubernetes cluster using `kubectl` command. The API (application programming interface) server determines if a request is valid and then processes it. the API is the interface used to manage, create, and configure Kubernetes clusters.
    
   	$ cat /etc/kubernetes/manifests/kube-apiserver.yaml  
	$ ps -aux |grep kube-apiserver

2). ETCD: 
-----------------------------------------------------------------
ETCD will stores data in key/value format. it is a distributed reliable key value store which stores the information of all the kubernetes resources such as nodes, Pods, replicasets, deployments, namespaces, configMaps, secrets, accounts, roles etc. In multi master environment it stores the data across mutiple master nodes, so all masters can have the information on all the k8s objects in a cluster. It also responsible to maintain the logs to avoid conflicts between masters. 

    $ cat /etc/kubernetes/manifests/etcd.yaml  

we must specify the certificate path location. the location of certificates are available in `/etc/kubernetes/pki`

    $ /etc/kubernetes/pki/etcd \
    -cacert /etc/kubernetes/pki/etcd/ca.crt \
    -cert /etc/kubernetes/pki/etcd/server.crt \
    -key /etc/kubernetes/pki/etcd/server.key 
    
    $ ps -aux |grep etcd
    
3). kube-Controller:
-----------------------------------------------------------------
kubernetes controller is a manager which controlls various k8s objects. controller is process which continuosly monitors various components of k8s inside the cluster. it is a brain to k8s cluster. it has various controllers inside to monitor different k8s objects. like node controller, deployment controller, namespace controller, job controller, replication controller, and etc.

Controller check the heart beat of the nodes, if the node heart beat(40sec) is not receive availabe then it mark that node as unreachable. similerly there are other controllers available in the controller manager.
       
       1. Node controller 	--> checks the node status and their avialability and the reach in the cluster
       2. Replication controller --> it will check the replicas defined are avaiable or not, if not will create the new pod
       3. namespace controller  --> namespace related activities are monitored by this 
       4. deployment controller
       5. endpoint controller
       6. job controller
       7. ServiceAccount controller
       8. Resource Quota controller
       9. Persistant volume controller

	$ cat /etc/kubernetes/manifests/kube-controller-manager.yaml --> controller YAML file is avaible here
 
	$ ps -aux | grep kube-controller-manager
   
4). kube-scheduler:
-----------------------------------------------------------------
kube-scheduler will deside which pod goes where, mean the scheduler will deside where to create the pod on which node, depends on the criteria. it will send the instructions to the `kubelet` agent on that node. scheduler just gives the instructions to the `kubelet`. `kubelet` is responsible to create the POD on that node.

    $ cat /etc/kubernetes/manifests/kube-scheduler.yaml
    $ ps -aux |grep kube-scheduler  --> process check 
    
5). kubelet: 
-----------------------------------------------------------------
`kubelet` is a agent running on all k8s cluster nodes. which is installed on all the `worker-nodes` and `master-nodes`. if we install kubernetes using `kubeadm`, it will not install kubelet in the nodes, we manually install the kubelet in the nodes. kubelet is in general defined as daemonset in k8s cluster. 

    download the installer and extract it as a servcie kubelet.service.d in path /etc/systemd/system/
    
    $ ps -aux |grep kubelet 	--> process check
    
6). kubeproxy:
-----------------------------------------------------------------
The `kube-proxy` is a network proxy service running on each node in a Kubernetes cluster. It's responsible for implementing Kubernetes networking concepts and managing network communication to facilitate connectivity between different pods and services within the cluster. `kube-proxy` also performs health checks on backend pods within Services to ensure that traffic is routed only to healthy pods capable of serving requests. For Services of type `LoadBalancer`, `kube-proxy` facilitates load balancing across multiple pods by distributing incoming traffic to the appropriate backend pods that comprise the Service.

`kubeproxy` is an agent with in the kubernetes cluster, every pod reaches every pod. this can be achived with POD networking. POD network is an internal network that spans accross all the nodes in the cluster to which all the pods in the cluster. 

kubeproxy is a process runs on each node in k8s cluster. its job is to look for new services. and everytime a new service is created, `kubelet` creates appropriate rule on each node to forward the trafic to those services to the backend pods. it does with `ip table` rules. `kube-proxy` run as daemonset in the k8s cluster.

    kube-proxy.service
    
    $ kubectl get pods --namespace kube-system 	--> to list the pods on namespace kube-system ( -n --> namespace)
    $ kubectl get daemonsets -n kube-system 	--> to list daemonsets.
    
7). Container Runtime:
-----------------------------------------------------------------
container runtime is software which used to create containers. like docker.

Kubernetes Objects: `explain`
-----------------------------------------------------------------
kubernetes resources are called as kubernetes objects. which are used to setup the kubernetes for application deployment.

	$ kubectl api-resources 			--> to list all kubernetes objects 
	$ kubectl api-resources --namespaced=true 	--> k8s objects which are created inside namespace
	$ kubectl api-resources --namespaced=false 	--> k8s objects which are not created inside namespace

  | Objects Created in-side NameSpace 	| Objects Created Out-Side NameSpace 	|
  | ------------------------------------|---------------------------------------|
  | POD					|					|
  | 
	1. Pod		 	 --> Is the smallest object is k8s. conatiner are created inside the Pods. 
	2. ReplicaSet 		 --> to replicate POD's on k8s cluster. and make sure defined number of replicas avaialbe. 
	3. ReplicationController --> same as replicaset, but replicaset is advanced.
	4. Deployment		 --> to deploy the application using deployment..
	5. Service 		 --> to expose the deployed services to the external network need to create services. 
	6. Daemonsets		 --> this pods created one for each cluster node. means one pod on one physical server.
	7. Namespace		 --> k8s cluster have mutiple namespaces, we devide namespaces as a working are for DEV/UAT/PROD.
	8. bindings
	9. configmaps                  
	10. endpoints                   
	11. events                      
	12. limitranges                 
	13. persistentvolumeclaims                            
	15. podtemplates                
	16. replicationcontrollers      
	17. resourcequotas              
	18. secrets                     
	19. serviceaccounts             
	21. daemonsets                  
	22. deployments                 
	23. replicasets                 
	24. statefulsets                   
	25. cronjobs                    
	26. jobs                                                   
	28. ingresses                   
	29. networkpolicies             
	31. rolebindings                
	32. roles    


**`kubectl`** : is a command, users use this command to interact with k8s cluster.

    $ kubectl version --> to check the kubernetes version
    
    $ kubectl explain <k8s-Object-name> 	--> Documentation for resource object (Pod, Replicaset, Deployment, service, etc.)
    $ kubectl explain pod 			--> Documentation for POD. to check apiVersion of k8s object.
    $ kubectl explain replicaset/rs 		--> Documentation for Replicaset
    $ kubectl explain replicationcontroller/rc 	--> doc. for RC
    $ kubectl explain deployment/deploy 	--> doc for deployment
    $ kubectl explain service/svc 		--> doc for service

Kubernetes objects are created in two types.

    1. imperative 	--> objects created using command line are called imperative.
    2. declarative 	--> objects created using yml/programing are called declarative.

Container:
-----------------------------------------------------------------
Containers are lightweight object, it pack your application code together with dependencies such as base OS, runtimes and libraries required to run your software services. containers are isolated evironments which act as indipendent system. container application exposes its services using a port know as containerPort. 

POD: (create, replace, delete, describe, explain, edit, run )
-------------------------------------------------------------------------------------------
pod is the smallest object in the k8s. the containers are run inside the Pod. In k8s each pod is represented as one host and each pod is allocated with one IP address. once the pod is destroyed, its containers & its ip is also removed. if new pod is created in place of old pod they will be allocated with new IP address which is allocated by the kubernetes. kubernetes pod are communicated with help of pod networking. its is an internal network with in the cluster.  

Syntax: Imperative way 
------------------------
kubectl run command is used only for pods. it used to create POD using command-line interface.
    
    $ kubectl run <Pod-Name> --image=<Pod-image-name>  

    $ kubectl run nginx --image=nginx --> creating a nginx pod
    $ kubectl run redis --image=redis --> creating a redis pod
    
Syntax: Declarative way
------------------------

pod-def.yaml 
-----------------------------------------------------------
```YML
---
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx-pod
    image: nginx
    ports:
    - containerPort: 80
    resources:			# To avoid the resource, we added this resources section. 
      requests:
        cpu: "100m"   		# Request 0.1 CPU cores
        memory: "128Mi"  	# Request 128 MiB memory
      limits:
        cpu: "500m"  	 	# Limit to 0.5 CPU cores
        memory: "256Mi"  	# Limit to 256 MiB memory
...
```
-------------------------------------------------------------
Note: In the above POD creation, we added the resources section to avoild the resouce starvation, this means By setting appropriate resource requests and limits, Kubernetes can efficiently manage resource allocation and prevent one application from consuming an excessive amount of resources, thereby avoiding starvation for other processes running in the cluster. Adjust these values based on your application's resource needs and cluster capacity.

create:
---------------
    	$ kubectl create -f pod-def.yaml  		--> create a pod using YAML file.
	$ kubectl replace -f pod-def.v2.yml  		--> updating with new version
get:
---------------	
	$ kubectl get pods        			--> to list pods running on "default" namespace
	$ kubectl get pods -o wide   			--> to list pods with aditional details
	$ kubectl get pods  -w    			--> to watch the pod status on fly
	$ kubectl get all         			--> to list all objects (Pod, Replicasets, Deploylments, services, etc.) running in default namespace
	$ kubectl get pods -n prod1-namespace 		--> to list PODs on namespace "prod1-namespace". insted of -n we can use --namespace
	$ kubectl get all --namespace=kube-system    	--> to list "kube-system" namespace objects, kubernetes object namespace
	$ kubectl get pods --show-labels 		--> show labels of all pods
	$ kubectl get pods --selector=env=prod 		--> fillter pods using the labels
 
    	$ kubectl get pods -n kube-system --show-labels | grep k8s-app=kube-dns --> filter the pods from 100's of pods

describe:
---------------
	   $ kubectl describe pod my-pod   		--> it will provide pod information
edit:
---------------
	   $ kubectl edit pod my-pod        		--> to edit the Pod on fly
delete:
---------------
	   $ kubectl delete pod my-pod  		--> to delete the pods

     	   	-o wide 		--> more details 
     		-n 			--> namespace 
       		-w			--> watch PODs 
	 	--show-labels		--> labels of PODs
   		--selector		--> filter the POD using labels
   
-------------------------------------------------------------------------------------------
NameSpace:
-------------------------------------------------------------------------------------------
K8s uses `Namespaces` to provides a mechanism for isolating groups of resources within a single cluster. we can create namespaces for each environment like Dev, Test, Prod. This will help in differentiate the objects of each env. object Names should be unique within a namespace, but not across namespaces. 

	By default we see 4 namespaces in K8S cluster. 

	 	1. kube-system
	  	2. kube-public
	   	3. kube-node-lease
	    	4. default
	
kube-system 	--> Kubernetes system components that are essential for the functioning of k8s. object like etcd, api-server, controller, kube-scheduler, kubelet and etc are created in this namespace. 
default 	--> Default namespace for users to use. It's a common place for deploying applications and services 
kube-public 	--> Intended for resources that should be made accessible publicly throughout the cluster. It's often used for resources that should be readable by all users.
kube-node-lease --> This namespace contains node lease objects, which are used to determine the availability of nodes in the cluster. `kubelet` to send heartbeats so that the **Master-Node** can detect node failures.

	$ kubectl api-resources --namespaced=true	--> resources which are created inside namespace
	$ kubectl api-resources --namespaced=false	--> resources which are created outside namespace
 
      	$ kubectl get namespaces			--> List namespaces available in cluster 

      	$ kubectl create namespace my-space1		--> Create a Namespace
      	$ kubectl describe namespace my-space1		--> show details of namespace
      	$ kubectl delete namespace my-space1		--> Delete the namespace
      	
       	$ kubectl config set-context --current --namespace=kube-system	--> switch between namespaces. 
	
-------------------------------------------------------------------------------------------
ReplicationController: (create, replace, delete, describe, explain, edit, apply )
-------------------------------------------------------------------------------------------
A `ReplicationController` ensures that a number of pod replicas are running. `ReplicationController` is similer to `ReplicaSet`. but `Replicaset` is the next-generation for `ReplicationController` that supports the new set-based label `selector`. It's mainly used by Deployment as a mechanism to orchestrate pod creation, deletion and updates. 

Note:  that we recommend using Deployments instead of directly using ReplicaSets, unless you require custom update orchestration or don't require updates at all.

rc-def.yaml
------------------------------------------------------------------------------------------------
```YML
---
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx
spec:
  replicas: 3 
  selector:
    app: nginx 
  template:
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80     # port defined in the container to expose 
```
----------------------------------------------------------------------------------------------

 	$ kubectl explain rc
  
	$ kubectl create -f rc-def.yaml --> create using YAML file. (or)
	$ kubectl apply -f rc-def.yaml
	
	$ kubectl get replicaitoncontroller (or)
	$ kubectl get rc
	
	$ kubectl delete rc my-rc1
	$ kubectl describe rc my-rc1
	$ kubectl edit rc my-rc1
	$ kubectl replace -f replicationcontroller-definiton.v2.yaml
	
-------------------------------------------------------------------------------------------
Replicaset: (create, replace, delete, describe, explain, edit, apply, scale, autoscale )
-------------------------------------------------------------------------------------------
A `ReplicaSet` purpose is to maintain a set of replicas of Pods running at any given time. `ReplicationController` and `ReplicaSet` are used for similer functionality. ReplicationController is older version, ReplicaSet is the newer version. In deployment k8s uses replicasets to replicate the pods. 

Note: ReplicaSet can own a non-homogenous set of Pods. it means if the `matchLabels` is condition is met with other pods the pods get destroyed by this replicaset controller to maintain the desired count. so careful while defining the labels. 

replicaset.v1.yaml
-------------------------------------------------------------------------------------------
```YML
apiVersion: apps/v1
kind: ReplicaSet
metadata:
    name: my-rs
    labels:
        app: my-rs-nginx
spec:
    replicas: 3
    selector:           
        matchLabels:
            app: nginx    
    template:
        metadata: 
            name: nginx-pod
            labels:
                app: nginx
        spec: 
            containers:
            - name: nignx
              image: nginx
              ports:
              - containerPort: 80
	      resources:		# To avoid the resource, we added this resources section. 
	        requests:
		  cpu: "100m"  	 	# Request 0.1 CPU cores
		  memory: "128Mi"  	# Request 128 MiB memory
	        limits:
		  cpu: "500m"   	# Limit to 0.5 CPU cores
		  memory: "256Mi"  	# Limit to 256 MiB memory
```
-----------------------------------------------------------------------------------

        $ kubectl create -f replicaset.v1.yaml --dry-run=client 	--> to run the YAML file with out applying the changes
        $ kubectl create -f replicaset.v1.yaml   			--> create replicaset

        $ kubectl get replicaset    			--> list replicasets
	$ kubectl get rs    				--> list replicasets 

        $ kubectl describe replicaset my-rs         	--> describe replicaset properties
        $ kubectl replace -f replicaset.v2.yaml     	--> replace the new app version with latest version
        $ kubectl scale replicaset my-rs --replicas=10  		--> scale up number of replicas 
        $ kubectl scale -f replicaset.yaml --replicas=10    		--> scale up number of replicas using replicaset definition file
        $ kubectl scale -f replicaset.yaml --replicas=2     		--> scale down number of replicas
       
        $ kubectl edit replicaset my-rs   			--> edit the replicaset properties using the 
        $ kubectl explain replicaset|grep -i version		
       
        $ kubectl delete replicaset my-rs   			--> to delete replicaset my-rs

        $ kubectl autoscale rs frontend --max=10 --min=3 --cpu-percent=50 	--> to autoscale replicas to desired 

ReplicaSet as a Horizontal Pod Autoscaler:(HPA)
------------------------------------------------
A ReplicaSet can also be a target for **Horizontal Pod Autoscalers** (HPA). That is, a ReplicaSet can be auto-scaled by an HPA.

**HPA:** (Horizontal Pod Autoscaling)
------------------------------------------------
The Horizontal Pod Autoscaler automatically scales the number of pod replicas in a deployment, replication controller, or replica set based on observed CPU utilization or custom metrics. For example, if the CPU usage of existing pods exceeds a certain threshold, HPA will automatically add more pods to distribute the load. Horizontal pod autoscaling does not apply to objects that can't be scaled (for example: a DaemonSet.). 

**VPA:** (Vertical Pod scaling)
------------------------------------------------
The Vertical Pod Autoscaler automatically adjusts the CPU and memory resource requests for a pod based on its usage patterns. For instance, if a pod is consistently using more CPU or memory than initially requested, VPA can dynamically increase the resource requests for that pod to ensure it has enough resources to operate efficiently.

HPA.yaml
-------------------------------------------------
```YML
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: my-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app			# HPA applying for Deployment "my-app"
  minReplicas: 1
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50

```
-------------------------------------------------

Note: autoscaling can be done using the command `autoscale` or as shown above we can use the hpa-rs.yaml

VPA.yml
---------------------------------------
```YML
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: my-app-vpa
spec:
  targetRef:
    apiVersion: "apps/v1"
    kind:       "Deployment"
    name:       "my-app"
  updatePolicy:
    updateMode: "Auto"
```

---------------------------------------------------------------------------------------------------------------
Deployment: (create, replace, delete, describe, explain, edit, apply, scale, autoscale, rollout, set, expose )
---------------------------------------------------------------------------------------------------------------
A Deployment provides declarative ways to  manage, create and scaling of identical pods, ensuring the desired state of an application.  Deployments can scale PODs, enable rollout of old code & update new code in a controlled manner. roll-back to an earlier deployment version if necessary. Do not overlap "labels and selectors" with other controllers (including other Deployments and StatefulSets). Kubernetes doesn't stop you from overlapping, and if multiple controllers have overlapping selectors those controllers might conflict and behave unexpectedly.

deployment updates can be done in 2 ways
            
            1). rollingupdate
            2). recreate 

by default k8s deployment will use rolling-update as default strategy.

        $ kubectl create deployment my-deploy --image=nginx --> creating a deployment with single POD
        $ kubectl create deployment my-deploy --image=nginx --replicas=6 --> create a deployment with 6 pod in cluster
        $ kubectl create deployment my-deploy --image=nginx --replicas=6 --dry-run=client -o yaml > my-deploy.yaml --> it will create a yaml of that deployment. 
        
deployment.yaml --> this deployment YAML file creates PODs, Replicasets, Deployment objects
-------------------------------------------------------------------------------------------------------
```YML
apiVersion: apps/v1
kind: Deployment
metadata: 
    name: my-deploy
spec:
    replicas: 5
    strategy:
    	type: RollingUpdate
    	rollingUpdate:
	      maxSurge: 25%   
      	      maxUnavailable: 25%
    selector:
        matchLabels:
            app: nginx
    template:
        metadata:
            name: nginx-pod
            labels:
                app: nginx
        spec:
            containers:
            - name: nginx
              image: nginx
              ports:
              - containerPort: 80
	      resources:		# To avoid the resource, we added this resources section. 
	        requests:
		  cpu: "100m"   	# Request 0.1 CPU cores
		  memory: "128Mi"  	# Request 128 MiB memory
	        limits:
		  cpu: "500m"   	# Limit to 0.5 CPU cores
		  memory: "256Mi"  	# Limit to 256 MiB memory
```
------------------------------------------------------------------------------------------------------

    $ kubectl create -f deployment-definition.yaml --dry-run=client 	--> to trial run the YAML file, if will not apply any changes
    $ kubectl create -f deployment-definition.yaml  			--> to exicute the YAML file.
    
    $ kubectl apply -f deployment-definition.yaml   --> to create/update using YAML files
    
    $ kubectl get deployment    		--> to check deployments available
    $ kubectl get deploy 			--> to check deployments available
    $ kubectl get deployment my-deply       	--> to check one deployment my-deploy
    $ kubectl get deployment --namespace=dev    --> to check the deploymets running on namespace "dev"
    
    $ kubectl describe deployment my-deploy 
    $ kubectl delete deployment my-deploy
    $ kubectl edit deployment my-deploy	--record  	--> update the version by editing the running deployment. --record will updated revision history

deployment rollout is done in two ways 

    1. RollingUpdate (Default) 	--> it will bringdown one by one depends on RollingUpdateStrategy defined. 
    2. Recreate 		--> will bringdown all pods at a time, and brinup all 
    
-------------------------
```
...
spec: 
  replicas: 5
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 25%   
      maxUnavailable: 25%
  selector:
  template:

....
```
--------------------------
    
Note: "Pod-template-hash" Do not change this label. This label ensures that child ReplicaSets of a Deployment do not overlap.

    $ kubectl apply -f deployment-definition.v2.yaml    		--> to update the latest version of application.
    $ kubectl replace -f deployment-definition.v2.yaml  		--> update new version(nginx to nginx:1.16.2) using YAML and replace running deployment.
    $ kubectl replace --force -f deployment-definition.v2.yaml  	--> bring down the existing deployment forcefully and creates the new one.
    
    $ kubectl set image deployment my-deploy nginx=nginx:1.16.1 	--> to set the new nginx image on the running deployment use <image name>=<image>

    $ kubectl rollout history deployment my-deploy  		--> history of the number of deployments
    $ kubectl rollout status deployment my-deploy   		--> deployment status can be checked. 
    $ kubectl rollout restart deployment my-deploy  		--> to restart the resources. 
    $ kubectl rollout undo deployment my-deploy     		--> if update is failed  then rollout the new-version to the old-verison 
    
    $ kubectl rollout undo deployment/nginx-deployment --to-revision=2 					--> rollback to a specific revision with --to-revision
    $ kubectl annotate deployment my-deploy1 kubernetes.io/change-cause="image updated to 1.16.1" 	--> update rollout history 
    
Note: we can see revision version details in the deployment description in annotations field.  By default, 10 old ReplicaSets will be kept, however its ideal value depends on the frequency and stability of new Deployments.

example: o/p
-------------------------------------------
```
  $ kubectl describe deployments.apps nginx-deployment 
    Name:               nginx-deployment
    Namespace:          default
    CreationTimestamp:  Thu, 26 May 2022 11:56:30 +0000
    Labels:             app=nginx
    Annotations:        deployment.kubernetes.io/revision: 2 # the new rollout version details. 
    Selector:           app=nginx
    Replicas:           3 desired | 3 updated | 3 total | 3 available | 0 unavailable
    StrategyType:       Recreate
    MinReadySeconds:    0
```
---------------------------------------------

Note:  you are running a Deployment with 10 replicas, maxSurge=3, and maxUnavailable=2. RollingUpdate Deployments support running multiple versions of an application at the same time. When you or an autoscaler scales a RollingUpdate Deployment that is in the middle of a rollout (either in progress or paused), the Deployment controller balances the additional replicas in the existing active ReplicaSets (ReplicaSets with Pods) in order to mitigate risk. This is called proportional scaling.

    $ kubectl scale deployment my-deploy --replicas=10  	--> to scale up the deployment
    $ kubectl scale deployment my-deploy --replicas=3   	--> to scale down the deployment
    $ kubectl autoscale deployment/nginx-deployment --min=10 --max=15 --cpu-percent=80
    
Note: remember every time new-verison changed, respective "replicaset" is brought down parallel a new "replicaset" is created with new-verison, the old replicasets. will avaiable but pods will not be avaiable in running state. when you do the undo operation thet old replicaset is recreated and new one will go down.

Q. How to deploy latest version of code on running deployment?
---------------------------------------------------------------
scenario-1: we can directly set the latest deployment code image using "set" command as below

    $ kubectl set deployment my-deploy nginx=nginx:1.16.1  busybox=busybox:1.12 
    
Scenario-2: we can edit the deployment using the "edit" command 

    $ kubectl edit deployment my-deploy 	--> it will open the running deployment in editing mode and will update the latest version
    
Scenario-3: using the deployment .yaml file and update the latest version and apply the changes

    $ kubectl apply -f deploy.yml

Q. How do you rollback to a specific version on the deployment?
-------------------------------------------------------------------
this can be achived using the rollout command with option --to-revision, and we can move to specific version

    $ kubectl rollout undo deployment my-deploy --to-revision=2 
    
Q. How to check the deployment status and history of deployments?
------------------------------------------------------------------
    $ kubectl rollout status deployment my-deploy
    $ kubectl rollout history deployment my-deploy


---------------------------------------------------------------------------------
Service: (create, replace, delete, describe, explain, edit, apply,
---------------------------------------------------------------------------------
In Kubernetes, a Service is a method for exposing an application services running on one or more Pods. Services offer a stable endpoint by grouping pods based on labels and facilitating load balancing and automatic discovery of pods, ensuring the application is available for access. for each servcie in k8s an IP is allocated to it. The set of Pods targeted by a Service is usually determined by a selector that you define in POD definition as labels.

    Publishing Services K8s services to external network is exposed in 3 ways 
    
        1). Cluster-IP (Default) 	--> Service only reachable within the cluster
        2). NodePort 			--> Exposes the Service on each Node's static port. port range 30000 - 32767
        3). LoadBalancer 		--> Exposes the Service externally using a cloud provider's load balancer.

 	$ kubectl create service nodeport my-service --node-port 31200 --tcp 5432:80 		--> Nodeport service creation
  	 
NodePort: service-nodeport.yml
--------------------------------
```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort   	#--> NodePort/ClusterIP/LoadBalancer
  selector:
    app: MyApp      	./'/#--> this is the MyApp Pod's label app=MyApp
  ports:
    - name: http
      port: 80  		#--> service exposes on this port
      targetPort: 9376 		#--> container exposed on this port
      nodePort: 32001  		#--> nodePort is on node, Port range from ( 30000-->32767)
    - name: https
      port: 443
      targetPort: 9377
      nodePort: 32002
```      
--------------------------------

Note: By default for convenience, the `targetPort` is set to the same value as the `port` field. you didn't specify the targetPort, system will consider port & targetPort both are same. 

ClusterIP: 
----------------------------------
Cluster-IP is a type of k8s service that is only reachable with in the k8s cluster. This is the Default service type.  by using `expose` command we expose an application services externally. this will create an "Endpoints" for your application to access. You can see the endpoints using `$ kubectl describe service <service-name>`

	$ kubectl expose deployment my-deploy --port=80 	--> this exposes the deployment as a clusterIP. to access URL use endpoints.
    	$ curl http://10.101.51.194 				--> to access the URL use clusterIP ip address

NodePort:   
-----------------------------------
NodePort Service will expose your application services via Hostport. NodePorts are ranges from port 30000 to 32767. To expose your application service 
 
    $ kubectl expose deployment my-deploy --type=NodePort --port=80 --> expose it as NodePort, to access URL use nodeport with ip. 
    $ curl http://localhost:31358 --> to access the URL use Node ip address and the NodePort

Note: remember the application listen port is defined with --port argument, should be same as --port other wise we can't access the application.

-------------------------------------
```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007
```      
--------------------------------------

LoadBalancer:
--------------------------------
A LoadBalancer service in Kubernetes is used to expose application services externally by provisioning a cloud-based load balancer. It distributes incoming traffic among pods associated with the service and provides an external IP for accessing the application from outside the cluster, making services accessible over the internet. The cloud provider allocates an external IP, directing traffic to the service and enabling public access to applications like web servers or APIs.

-----------------------
```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: LoadBalancer	
  selector:
    app: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
  clusterIP: 10.0.171.239
status:
  loadBalancer:
    ingress:
    - ip: 192.0.2.127
```    
---------------------------------------

        $ kubectl create -f sevice-definition.yaml 	--> to create the sevice
        $ kubectl get services  	--> to check the services
        $ kubectl get services -o wide 	--> to see more details 
        $ kubectl delete serivce my-svc --> to delete 
        $ kubectl edit service my-svc 	--> to edit the service properties

--------------------------------------------------------------------------------------------------------
NameSpace: (create, get, api-resources, config ) 
--------------------------------------------------------------------------------------------------------
In Kubernetes, namespaces provides a mechanism for isolating groups of resources(pods, replicasets, deployments, services, etc..) within a single cluster. Names of resources need to be unique within a namespace, but not across mutiple namespaces. In simple terms namespace is a isolated area for a team.

	$ kubectl get namespace 		--> to list the namespaces avaiable
	$ kubectl get namespaces --show-labels 	--> it will show labels
 
        $ kubectl api-resources --namespaced=true 	--> k8s objects which are created inside namespace
        $ kubectl api-resources --namespaced=false 	--> k8s objects which are not created inside namespace
        
Kubernetes starts with four initial namespaces:

    1). default: The default namespace for objects with no other namespace
    2). kube-system: The namespace for objects created by the Kubernetes system
    3). kube-public: This namespace is created automatically and is readable by all users 
    4). kube-node-lease: This namespace holds Lease objects associated with each node. it allow the "kubelet" to send heartbeats so that master node can detect node failure.
    
Note: `kubeconfig` file is avaiable in the $HOME/.kube/config 	--> this is the file which is visible when we exicute the below command.

        $ kubectl config view 		--> to see kubeconfig file.
        $ kubectl config get-contexts 	--> to check details for current namespace, cluster information
        $ kubectl config view | grep namespace  	--> to check the namespace currenlty we are working on. if we are default it won't show.
        
	$ kubectl run nginx --image=nginx --namespace=prod1 --> create nginx pod in prod1 namespace, prod1 namespace should be created.
        $ kubectl get pods --namespace=prod1 --> to list pods from namespace prod1
       
        $ kubectl config set-context --current --namespace=prod1 --> to switch from "default" to "prod1" namespace
        $ kubectl config set-context --current --namespace=kube-system --> switch to kubernetes namespace

When you create a Service, it creates a corresponding DNS entry. This entry is of the form `<service-name>.<namespace-name>.svc.cluster.local` which means that if a container only uses `<service-name>`, it will resolve to the service which is local to a namespace. This is useful for using the same configuration across multiple namespaces such as Development, Staging and Production. If you want to reach across namespaces, you need to use the fully qualified domain name (FQDN).

	$ kubectl create namespace prod1 --> this will create  namespace prod1

	
prod1-namespace-def.yaml
-------------------------
```	
apiVersion: v1
kind: Namespace
metadata:
    name: prod1
```
----------------------------------


nginx-prod1-definition.yaml --> this will create nginx pod in namespace prod1
-----------------------------------------------------------------------------------

```
apiVersion: v1
kind: Deployment
metadata:
    name: nginx-prod1
    namespace: prod1    # add namespace in metadata to create the deployment in that namespace
spec:
    replicas: 4
    selector:
        matchLabels: 
	  app: nginx
    template:
        metadata:
            name: nginx
        spec:
            containers:
            - name: nginx
              image: nginx

	      
```
------------------------------------------------------------

    $ kubectl create -f prod1-namespace-def.yaml --> this yaml file create namespace prod1
    $ kubectl create -f nginx-prod1-definition.yaml --> this will create nginx prod on namespace prod1

    $ kubectl get pods --namespace=prod1 --> to list the prods
    $ kubectl get namespace --show-labels --> to show labels of all namespaces

    $ kubectl delelte namespace prod1 --> to delete the prod1 namespace

    $ kubectl config get-context --> to check the namespace 
    
----------------------------------------------------------------------------------------- 
Namespace: ResourceQuota
-----------------------------------------------------------------------------------------
Resource quotas in Kubernetes allow cluster administrators to allocate and limit the amount of compute resources (CPU, memory, storage) and object count (pods, services, etc.) that can be used within a namespace. This helps in resource governance and prevents any single namespace from consuming excessive resources, thereby ensuring fair resource distribution among different teams or applications.

there are 2 steps involved 

	1. Create a namespace 
 	2. Create a ResourceQuota for your namespace. 

Note: before creating resource quota for namespace, we need to create namespace.

	$ kubectl create namespace myspace	--> create namespace
	$ kubectl create quota test --hard=count/deployments.apps=2,count/replicasets.apps=4,count/pods=3,count/secrets=4 --namespace=myspace	--> allocate quotas to the namespace
	$ kubectl create deployment nginx --image=nginx --namespace=myspace --replicas=2	
	$ kubectl describe quota --namespace=myspace

prod2-ns-resourcequota-def.yaml
-----------------------------------------------------------------------------
```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: my-ns-rquota
  namespace: prod2
spec:
  hard:
    requests.cpu: "1"
    requests.memory: 1Gi
    limits.cpu: "2"
    limits.memory: 2Gi
    requests.nvidia.com/gpu: 4
 ```   
-------------------------------------------------------------------------------

-------------------------------------------------------------------------------
nodeName: Manual scheduling
-------------------------------------------------------------------------------
kube-scheduler is responsible to devide and distribute the work among all the nodes equally, if a new node is created, it will allocate work to it. if you don't want a default scheduler to chose where to create pod on the cluster nodes, specifing the parameter/keyword "nodeName", to create pod on the defined Node name. every pod has a field called "nodeName" which is by default not set, the scheduler goes to all PODs and looks for this property, if not avaialbe the scheduler will run an algoritham and assign a node to this pod. 

if scheduler is down, we can't start the pod, it will go to pending state, to avoid this we can use `nodeName` parameter specifying in pod creation. this will only work during the pod creation, to change the pod nodeName during runtime we need to use Binding methord to change nodeName. 

    $ kubectl get nodes     --> to see the nodes in the cluster and its details.

pod-definition.yaml 
-----------------------------------------------------------------------------------
```
apiVersion: v1
kind: Pod
metadata: 
    name: nginx-pod
    labels:
        app: nginx
spec:
    nodeName: controlplane      # check the list on nodes avaiable and specify the name of the node
    containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 8080
```
------------------------------------------------------------------------------------     


pod-bind-runtime.yaml --> this file will change the pod nodeName during runtime. but below file need to convert into JSON format. pass it as a command line option.
-------------------------------------------------------------------------------------
```
apiVersion: v1
kind: Binding
metadata: 
    name: nginx-pod
target:
    apiVersion: v1
    kind: Node
    name: node01
```
    
------------------------------------------------------------------------------------
Labels & Selectors:
------------------------------------------------------------------------------------
Labels and selectors are the properties attached to each k8s objects(Pods, deployments, services, etc..). they use to group kubernetes objects/resources to gether. labels and annotations are attached in metadata section in YAML file. Labels can be used to select objects and to find collections of objects that satisfy certain conditions. Annotations are not used as labels they identify the object details like name, phone number, author, etc.

	$ kubectl run nginx --image nginx --labels env=dev,bu=finance,cn=IND	--> this will label the prod with 3 labels.
 	$ kubectl get pod --selector env=dev 	--> filter the pods with labels.
  
-----------------------------------------
```
apiVersion:v1
kind: Pod
metadata:
    name: my-pod
    labels:
        app: nginx
        env: prod
        tire: frontend
        bu: finance
```
-------------------------------------------

    $ kubectl get all --selector app=nginx      --> to filter the all k8s objects using selector fields.
    $ kubectl get pods --selector env=prod
    $ kubectl get pods --selector env=prod,app=nginx
    $ kubectl get pods --show-labels

    $ kubectl get nodes --show-labels       --> to show labels defined for a node.
    
---------------------------------------------------------------------------
NodeSelector:
---------------------------------------------------------------------------
NodeSelector is similer to nodeName, but nodeSelector uses node labels to schedule the pods. for example in a cluster few nodes, labeled as `disktype=ssd` then if we use this label with nodeSelector field. that means all the nodes with `disktype=ssd` are all eligible to create the pods in that node. to apply this property we need to follow 2 steps

step-1: Labeling the node :
---------------------------------------

    $ kubectl label node node01 env=prod	--> to create a label to node01 as env=prod
    $ kubectl label node node01 env-		--> delete the label using "-" 
    $ kubectl get node node01 --show-labels 	--> to show the labels of node01

step2: creating the pod with nodeSelector :
------------------------------------------

my-pod.yaml    
-----------------------------------------------
```
apiVersion: v1
kind: Pod
metadata:
    name: nginx-pod
spec:
    nodeSelector:
        env: prod
    containers:
    - name: nginx
      image: nginx
```
-----------------------------------------------

------------------------------------------------------------------------------------
Annotations:
------------------------------------------------------------------------------------
Annotations are used to record details like name, build, email, phonenumber, other information. this information is not used to filter the k8s objects. they are used for
information purpose only. 

------------------------------------
```
apiVersion: v1
kind: Pod
metadata:
    name: my-pod
    labels:
        app: nginx
    annotations:
        name: karunakar
        email: sample@xyz.com
        phone: 999999999
```
------------------------------------

------------------------------------------------------------------------------------
Taint & Tolerations:
------------------------------------------------------------------------------------
Taints and tolerations work together to ensure that pods are not scheduled onto inappropriate nodes. One or more taints are applied to a node. this marks that the node should not accept any pods that do not tolerate the taints. to allow the tained nodes to create pods, we need to create the pods with taint tolerations while creating the pod.

	Taint: is applied to nodes, it will reject the pods if the pods not has tolerations 
	Tolerations: is applied to pods, it will allow the pod to accept by the tainted node.

taints are 3 types

    1). NoSchedule : it will not allow pod to schedule on node, if already running it will ignore
    2). PreferNoSchedule : it will not allow pod on the node, if no choice it will ignore this property and allow pod to schedule
    3). NoExecute: it will not allow, if already any pod running it will remove them.
    
        $ kubectl taint node node01 env=UAT:NoSchedule  	
        $ kubectl taint node node02 env=dev:PreferNoSchedule	
        $ kubectl taint node node03 env=prod:NoExecute

        $ kubectl desribe node node01|grep taint 
        
        $ kubectl taint node node01 env=UAT:NoSchedule-  	--> this is to remove the taint from a node use "-" at the end. 

pod-tolaration.yaml
----------------------------------------------------------------
```
apiVersion: v1
kind: Pod
metadata: 
    name: nginx-pod
    labels:
        app: nginx
spec:
    containers:
    - name: nginx
      image: nginx
    tolerations:        # this pod will only get created on  node01 
    -   key: "env"
        operator: "Equal"
        value: "UAT"
        effect: NoSchedule
```
-----------------------------------------------------------------
```
spec:
    contianers:
    - name: nginx
      image: nginx
    tolerations:  # pod has two tolerations, depends on the scheduler selection criteria
    -   key: "env"
        operator: "Equal"
        value: "prod"
        effect: NoExecute
    -   key: "env"
        operator: "Equal"
        value: "UAT"
        effect: NoSchedule        
```
-----------------------------------------------------------------

-----------------------------------------------------------------------------------------	
Daemonset:
-----------------------------------------------------------------------------------------
In K8s DaemonSet ensure a specific pod runs on every node in a cluster. The primary purpose of a DaemonSet is to manage and maintain a set of pods, typically for background services, monitoring agents, or other system-level components that need to run on all nodes within the cluster. 

Key Features and Characteristics:

	One Pod Per Node:		--> A DaemonSet ensures that exactly one instance of a pod runs on each node in the cluster where the DaemonSet is applied.
	Automatic Scheduling: 		--> When new nodes join the cluster, a DaemonSet automatically schedules the required pods on those nodes, ensuring that each node has the specified pods running.
	Pod Lifecycle Management: 	--> DaemonSets handle pod lifecycle events, ensuring that pods are rescheduled or replaced if they fail or are deleted.
	Use Cases: 			--> DaemonSets are commonly used for tasks like log collection, monitoring, networking, storage daemons, or any service that should run on all nodes.

Example Use Cases:

	Monitoring Agents: 	--> Deploying monitoring agents like Prometheus Node Exporter to collect node-specific metrics from each node.
	Logging: 		--> Running logging agents (e.g., Fluentd, Filebeat) to collect logs from each node and forward them to a central logging system.
	Network Plugins: 	--> Deploying network plugins or proxies required on each node for networking purposes.

example: kube-proxy, kubelet, weave-net, 

Some typical uses of a DaemonSet are:

    1). running a cluster storage daemon on every node
    2). running a logs collection daemon on every node
    3). running a node monitoring daemon on every node

Daemonsets are like replicasets, the pod defined in daemonset, will create one pod on each node so that it can be avaiable on all nodes in a cluster. when a new node is added to the cluster daemonset will create one pod in new node, if the node is removed it will remove the pod on the node. this is useful when you want to monitor logs of each node (or) run services like kubeproxy on each node, (or) monitoring the node status. etc. in this scenario we use the daemonsets. 

kube-proxy-daemonset.yaml
-------------------------------------------------------------------------------
```
apiVersion: apps/v1
kind: DaemonSet
metadata:
    name: kube-proxy
    namespace: kube-system
    labels:
        k8s-app: kube-proxy
spec:
    selector:
        matchLabels:
            k8s-app: kube-proxy
    template:
        metadata:
            labels:
                k8s-app: kube-proxy
        spec:
            containers:
            - command:
                - /usr/local/bin/kube-proxy
                - --config=/var/lib/kube-proxy/config.conf
                - --hostname-override=$(NODE_NAME)
                ....
                ....
                ....
```
---------------------------------------------------------------
```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-ds-pod
  labels:
    app: nginx
spec:
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx
```
-------------------------------------------------

-----------------------------------------------------------------------------------------
Static PODs: (/etc/kubernetes/manifests | /var/lib/kubelet/config.yaml)
-----------------------------------------------------------------------------------------
Static Pods are managed directly by the `kubelet` daemon on a specific node, without the API server observing them. Kubelet daemon will monitor this directory `/etc/kubernetes/manifests/` and new pod definition files are exicuted. these pods are not controlled by `kubectl` but just we can monitor, if you want to delete those pods just have to remove the file from that location. this location can be changed according to over convenence using the file `/var/lib/kubelet/config.yaml`. change the `staticPodPath` we can't deploy objects like replicasets, deployments, sevices as a static pods

        $ ps -ef |grep kubelet 

Note: all static pods ends with node name 

simple-static-pod.yaml
-------------------------------
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx
```
--------------------------------

        $ kubectl get pods --all-namespaces
        $ cd /etc/kubernetes/manifests
        $ more /var/lib/kubelet/config.yaml

all kuberenetes components are placed as static pod onthe system in  `/etc/kubernetes/manifests`

        /etc/kubernetes/manifests ➜  ls -ltr
        -rw------- 1 root root 1450 Feb 19 10:07 kube-scheduler.yaml
        -rw------- 1 root root 3380 Feb 19 10:07 kube-controller-manager.yaml
        -rw------- 1 root root 3849 Feb 19 10:07 kube-apiserver.yaml
        -rw------- 1 root root 2215 Feb 19 10:07 etcd.yaml

Note: daemonset and static pods are ignored by kube-scheduler


-----------------------------------------------------------
NodeAffinity:
-----------------------------------------------------------------
Node affinity is conceptually similar to `nodeSelector`. it allows you to constrain which nodes your pod is eligible to be scheduled on, based on labels on the node. You can use `In, NotIn, Exists, DoesNotExist, Gt and Lt`.

there are 2 types of nodeAffinity 

    1). requiredDuringSchedulingIgnoredDuringExecution 
    2). preferredDuringSchedulingIgnoredDuringExecution

requiredDuringScheduling: The scheduler can't schedule the Pod unless the rule is met.
preferredDuringScheduling: The scheduler tries to find a node that meets the rule. If a matching node is not available, still schedules the Pod. You can specify a weight between 1 and 100 for each instance of the preferredDuringSchedulingIgnoredDuringExecution affinity type.
    
    except that it will evict pods from nodes that case to satisfy the pods' node affinity requirements.
    
Note: In the preceding types, `IgnoredDuringExecution` means that if the node labels change after Kubernetes schedules the Pod, the Pod continues to run.

Scenario-1: nodeSelector & affinity
-----------------------------------------------------------
Note: If you specify both nodeSelector and affinity, both must be satisfied for the pod to be scheduled onto a candidate node. the pod will get schedule on the node only both conditions are statisfied. ( nodeSelector & affinity rule)
 
pod-affinity-nodeSelector.yaml 
--------------------------------------------------------
```
apiVersion: v1
kind: Pod
metadata:
    name: nginx-pod
    labels:
        app: nginx
spec:
  nodeSelector:
    bu: finanace
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        -  matchExpressions:
            -   key: env
                operator: In     # we can use operators In/ NotIn/ Exists/ DoesNotExist/ Gt/ Lt
                values:          # the pod can be scheduled onto a node if one of the nodeSelectorTerms can be satisfied.
                - prod1
                - prod2
  containers:
    - name: nginx
      image: nginx
      ports:
      - port: 80
        targetport: 80
        nodePort: 30008
```
-----------------------------------------------------------

Scenario-2: nodeSelectorTerms
-----------------------------------------------------------
If you specify multiple "nodeSelectorTerms" associated with nodeAffinity types, then the pod can be scheduled onto a node if one of the nodeSelectorTerms is satisfied.

pod-affinity.yaml
-----------------------------------------------------
```
apiVersion: v1
kind: Pod
metadata:
    name: nginx-pod
    labels:
        app: nginx
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        -   matchExpressions:
            -   key: env
                operator: In/ NotIn/ Exists/ DoesNotExist/ Gt/ Lt
                value:  #  the pod can be scheduled onto a node if one of the nodeSelectorTerms can be satisfied.
                - prod1
                - prod2
      preferredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:  
        -  matchExpressions:
           - key: bu
             operator: In
             values:
             - finance 
             - business
  containers:
    - name: nginx
      image: nginx
```
------------------------------------------------------

Scenario-3: matchExpressions
------------------------------------------------------
If you specify multiple "matchExpressions" associated with nodeSelectorTerms, then the pod can be scheduled onto a node only if all matchExpressions is satisfied.

pod-affinity.yaml
-----------------------------------------------------
```
apiVersion: v1
kind: Pod
metadata:
    name: nginx-pod
    labels:
        app: nginx
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        -   matchExpressions:
            -   key: env 
                operator: In/ NotIn/ Exists/ DoesNotExist/ Gt/ Lt
                value:  #  the pod can be scheduled onto a node if one of the nodeSelectorTerms can be satisfied.
                - prod1
                - prod2
            -   key: bu
                operator: Exists
  containers:
  - name: nginx
    image: nginx

```
-----------------------------------------------------------------

Node affinity per scheduling profile:
-----------------------------------------------------------------
When configuring multiple scheduling profiles, you can associate a profile with a Node affinity, which is useful if a profile only applies to a specific set of Nodes. 

To do so, add an addedAffinity to the args of the NodeAffinity plugin in the scheduler configuration. 

------------------------------------------------------------------
```
apiVersion: kubescheduler.config.k8s.io/v1beta1
kind: KubeSchedulerConfiguration

profiles:
  - schedulerName: default-scheduler
  - schedulerName: foo-scheduler
    pluginConfig:
      - name: NodeAffinity
        args:
          addedAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
              nodeSelectorTerms:
              - matchExpressions:
                - key: scheduler-profile
                  operator: In
                  values:
                  - foo

```
-------------------------------------------------------------

Inter-pod affinity and anti-affinity:
-----------------------------------------------------------------

Inter-pod affinity and anti-affinity allow you to constrain which nodes your pod is eligible to be scheduled based on labels on pods that are already running on the node rather than based on labels on nodes.

Note: Inter-pod affinity and anti-affinity require substantial amount of processing which can slow down scheduling in large clusters significantly. We do not recommend using them in clusters larger than several hundred nodes.

------------------------------------------------------------------
```
apiVersion: v1
kind: Pod
metadata:
  name: with-pod-affinity
spec:
  affinity:
    podAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchExpressions:
          - key: security
            operator: In
            values:
            - S1
        topologyKey: topology.kubernetes.io/zone
    podAntiAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 100
        podAffinityTerm:
          labelSelector:
            matchExpressions:
            - key: security
              operator: In
              values:
              - S2
          topologyKey: topology.kubernetes.io/zone
  containers:
  - name: with-pod-affinity
    image: k8s.gcr.io/pause:2.0
```
-----------------------------------------------------------------

-----------------------------------------------------------------------------------
Resource Limits:
-----------------------------------------------------------------------------------
When a pod placed on a node it will use system resources such as CPU, Memeory, Disk Space. if node doesn't have the sufficient resources the kube-scheduler avoid placing the pod on the node it shows insufficient CPU. Kubernetes doesn't provide default resource limits. This means that unless you explicitly define limits, your containers can consume unlimited CPU and memory.

Note:  we can specify the resource requirement for each pod using resources parameter. it's useful to specify CPU units less than 1CPU or 1000milli using the milliCPU form. 	

default values k8s will consider as 0.5-CPU, 256mi-MEM, DISK

For the POD to pick up those default values, first you need to set default values by creating a `LimitRange` in namespace.


	1 CPU   = 1000m CPU (milli) 
	0.1 CPU = 100m CPU
	1Gi MEM = 1024Mi MEM
	1Mi MEM = 1024Ki
	1Ki MEM = 1024bytes

pod-def-resource-limit.yaml
------------------------------------
```
---
apiVersion: v1
kind: Pod
metadata:
	name: my-pod
	labels:
	  app: my-pod
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    	- containerPort: 80
    resources:
    	requests:
  	  memory: "1Gi"
	  cpu: 0.5
	limits:
	  memory: "2Gi"
	  cpu: 1
```
-----------------------

-----------------------------------------------------------------------------------
ResourceQuota: limiting the number of k8s objects for a namespace
-----------------------------------------------------------------------------------
It can limit the number of k8s objects/resources that can be created in a namespace. this can be done 
 
-------------------------
```
apiVersion: v1
kind: ResourceQuota
metadata:
   name: my-rq-limits
   namespace: my-namespace1
spec:
  hard:
    cpu: "1000"
    memory: "20Gi"
    pods: "10"
    configmaps: "10"
    secrets: "20"
    services: "30"/
 ```
---------------------------------------------

-----------------------------------------------------------------------------------------
LimitRange: Restrict POD resource usage limit at NameSpace level
-----------------------------------------------------------------------------------------
Pod resource usage can be restricted using LimitRange, this is the default values set at namespace level. when a Pod created it will by default restrict its limits as defined in the "LimitRange", so when POD usage exceeds its limits. it will not let the system use the system resouces. 

Note: pod default resource requirement need to set at the namespace level first. like CPU, Memory, DISK.
	
Memory limits: memory-limits.yaml
-----------------------------------------------------------------
```
apiVersion: v1
kind: LimitRange
metadata:
  name: resource-limits
  namespace: my-namespace1
spec:
  limits:
    - type: Container
      defaultRequest:	# Defines default request values for CPU and memory.
        cpu: "100m"
        memory: "256Mi"
      default:		#  Sets default limits for CPU and memory.
        cpu: "200m"
        memory: "512Mi"
      max:		# Specifies the maximum limits that a Container can request.
        cpu: "1"
        memory: "1Gi"
      min:		#  Specifies the minimum limits that a Container can request.
        cpu: "50m"
        memory: "128Mi"
    - type: PersistentVolumeClaim	# Defines constraints for PersistentVolumeClaims related to storage.
      max:
        storage: "5Gi"
```
---------------------------------------------------------------------   

        $ kubectl apply -f memory-limits.yaml --namespace=prod1

CPU-Limits: cpu-limits.yaml
---------------------------------------------------------------------
```
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
spec:
  limits:
  - default:
      cpu: 1
    defaultRequest:
      cpu: 0.5
    type: Container
```
------------------------------------------------------------------------

        $ kubectl apply -f cpu-limits.yaml --namespace=prod1 
        $ kubectl get pod default-mem-demo --output=yaml --namespace=prod1 --> to view detailed information.

What if you specify a container's limit, but not its request? 
----------------------------------------------------------------
```
apiVersion: v1
kind: Pod
metadata:
  name: default-mem-demo-2
spec:
  containers:
  - name: default-mem-demo-2-ctr
    image: nginx
    resources:
      limits:
        memory: "1Gi"
```
--------------------------------------------------------------

What if you specify a container's request, but not its limit?
--------------------------------------------------------------
the container's memory request is set to the value specified in the container's manifest. The container is limited to use no more than 512MiB of memory, which matches the default memory limit for the namespace.

--> Remember, you CANNOT edit specifications of an existing POD other than the below.

	spec.containers[*].image
	spec.initContainers[*].image
	spec.activeDeadlineSeconds
	spec.tolerations

What is the difference between `ResourceQuota` vs `LimitRange`?
--------------------------------------------------------------
LimitRange and ResourceQuota are objects used to control resource usage by a Kubernetes cluster administrator. "ResourceQuota" is for how many k8s objects are eligible to create inside the namespace. where as "LimitRange" is for how much system resouces are allocated for each resource like "cpu", "memory", "storage".

ResourceQuota is for limiting the total resource consumption of a namespace, for example:
-------------------------------
```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: object-counts
spec:
  hard:
    configmaps: "10" 
    persistentvolumeclaims: "4" 
    replicationcontrollers: "20" 
    secrets: "10" 
    services: "10"
 ```   
----------------------------------

LimitRangeis for managing constraints at a pod and container level within the project.
----------------------------------------
```
apiVersion: "v1"
kind: "LimitRange"
metadata:
  name: "resource-limits" 
spec:
  limits:
    -
      type: "Pod"
      max:
        cpu: "2" 
        memory: "1Gi" 
      min:
        cpu: "200m" 
        memory: "6Mi" 
    -
      type: "Container"
      max:
        cpu: "2" 
        memory: "1Gi" 
      min:
        cpu: "100m" 
        memory: "4Mi" 
      default:
        cpu: "300m" 
        memory: "200Mi" 
      defaultRequest:
        cpu: "200m" 
        memory: "100Mi" 
      maxLimitRequestRatio:
        cpu: "10"
```
-------------------------------------
An individual Pod or Container that requests resources outside of these LimitRange constraints will be rejected, where as a ResourceQuota only applies to all of the namespace/project's objects in aggregate.


-------------------------------------------------------------------------------------------------------
Multiple Schedulers:
-------------------------------------------------------------------------------------------------------
Kubernetes ships with a "default scheduler" that is described here. If the default scheduler does not suit your needs you can implement your own scheduler. Moreover, you can even run multiple schedulers simultaneously alongside the default scheduler and instruct Kubernetes what scheduler to use for each of your pods.

default-scheduler.yaml  
----------------------------------------------------------------
```
apiVersion: kubescheduler.config.k8s.io/v1alpha1
kind: KubeSchedulerConfiguration
profiles:
- schedulerName: default-scheduler
  plugins:
    score:
      disabled: false
    preemption:
      disabled: false
  # Add other configuration specific to this scheduler...

```

custom-scheduler.yaml
----------------------------------------------------------------
```
apiVersion: kubescheduler.config.k8s.io/v1alpha1
kind: KubeSchedulerConfiguration
profiles:
- schedulerName: custom-scheduler
  plugins:
    score:
      disabled: false
    preemption:
      disabled: false
  # Add other configuration specific to this scheduler...

```

	$ kube-scheduler --config=default-scheduler.yaml
	$ kube-scheduler --config=custom-scheduler.yaml

-------------------------------------------------------------------------------------------------------
Monitoring:
-------------------------------------------------------------------------------------------------------
we like to monitor node level metrix such as resource consumtion on k8s,  on node level metrics, how many nodes in the cluster ? how many are healthy? performance metrics like CPU, MEMORY, DISK, Network utilization. Pod level metrix such as CPU consumtion for each pod performancd and etc. by default k8s not comes with monitoring solution. 

Key Aspects of Monitoring:
--------------------------------
	1. Cluster Metrics: 	Monitor the overall health and performance of the Kubernetes cluster, including resource utilization, node health, and API server responsiveness.
	2. Pod Metrics: 	Track pod-specific metrics such as CPU and memory usage, restarts, and network activity to ensure they are running as expected.
	3. Logs and Events: 	Collect and analyze logs and events generated by pods, nodes, and the Kubernetes system to diagnose issues and troubleshoot problems.
	4. Alerting and Notifications: 	Set up alerts based on predefined thresholds or abnormal behaviors to proactively detect and respond to issues.

Popular Monitoring Tools 
------------------------
	1. Metrix severs --> memroy monitoring solution.
	2. prometheus	--> An open-source monitoring and alerting toolkit that collects and stores metrics data
 	3. Grafana	--> A visualization tool that works well with Prometheus for creating dashboards and visual representations of monitored data.
  	4. Logs		--> ELK Stack (Elasticsearch, Logstash, Kibana), Fluentd, and Loki are commonly used for log collection, aggregation, and analysis within Kubernetes clusters.
   
	5. elastic stack
	6. Datadog
	7. dynatrace
    
------------------------------------------------------------------------------------------------------
Metrix server: monitoring utility
------------------------------------------------------------------------------------------------------
we can have 1 metrix sever for each k8s cluster. it is an in memory monitroing solution. we can't see historical performace. so we need to relay on other softwares for this.  k8s run an agent on each node is kubelet. kubelet also contain a sub component called cAdviser(container Adviser). which is resposible collecting the node metrix and send it to the metrics sever.

for minikube, we can use 

        $ minikube addons enable metrics-server

for other setups

        $ git clone https://github.com/kubernetes-incubator/metrics-server
        $ kubectl create -f ... options.

we can view the metrics details using the below

        $ kubectl top node 
	$ kubectl top pod

---------------------------------------------------------------------------------------------------
Logging:
---------------------------------------------------------------------------------------------------
to view the Live logs of the pod use -f option, we can view the logs during the execution.

        $ kubectl logs -f <pod-name> 			--> if we have only one pod in a definition file. this methord will work.
        $ kubectl logs -f <pod-name> <container-name> 	--> if multiple containers are available in the pod.

------------------------------------------------------------------------------------------------------
Configure Applications:
------------------------------------------------------------------------------------------------------
Configuring applications comprises of understanding the following concepts:

	1. Configuring Command and Arguments on applications
	2. Configuring Environment Variables
	3. Configuring Secrets

1. Configuring Command and Arguments on applications:
---------------------------------------------------------------------------------------------------
we use the docker images in k8s. so when we use the images where it require inputs how are we pass them using `command` and `args`.  The command and arguments that you define cannot be changed after the Pod is created. docker uses 2 parameters in docker images.

	1. $ docker run ubuntu-sleep sleep 10 
    	2. ENTRYPOINT ["sleep"] 	--> it will look for the input, $ docker run ubuntu-sleeper 10

to pass the arguments similer in the k8s, we use this options
------------------------------------------------------------------------
```
apiVersion: v1
kind: Pod
metadata:
  name: command-demo
  labels:
    purpose: demonstrate-command
spec:
  containers:
  - name: command-demo-container
    image: debian
    command: ["printenv"]			# Docker ENTRYPOINT is override with command in k8s
    args: ["HOSTNAME", "KUBERNETES_PORT"]	# docker CMD will override with args in k8s
  restartPolicy: OnFailure

```
--------------------------------------------------------------------


Configuring Environment Variables:
-------------------------------------------------------------------------------------------------
if the docker container require any environment variable we specify them with option "-e key=value". similarly in k8s we specify the environment variables like below. we can also specify the env variables in k8s as configMaps or secrets. 

pod-definition.yaml
--------------------------------------------------
```
apiVersion: v1
kind: Pod
metadata:
    name: nginx-pod
    labels:
        app: nginx
spec:
    env: 
    - name: APP_COLOR
      value: Red
    - name: APP_ANIMAL
      value: cat
    - name: APP_SIZE
      value: large
    - name: APP_MODE
      value: prod

```    
---------------------------------------------------

-------------------------------------------------------------------------------------------------
ConfigMaps: 
-------------------------------------------------------------------------------------------------
How to create & Inject configMap in k8s ? A `ConfigMap` is an K8S object used to store **non-confidential** data in **key-value** pairs. Pods can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a volume. A ConfigMap is not designed to hold large chunks of data. The data stored in a ConfigMap cannot exceed 1MiB. If you need to store larger chunks of data, you may want to consider mounting a `volume` or use a separate database or file service.

Note: The spec of a static Pod cannot refer to a ConfigMap or any other API objects. The Pod and the ConfigMap must be in the same namespace.

there are 2 steps involved in setting up the configmaps.

    step-1. create configMap
    step-2. inject configMap

Scenario-1: Creating configMap and Injecting complete configMap in a main YAML file.
-------------------------------------------------------------------------------------

Step-1: Create a configMap 

 	$ kubectl create configmap mysql-config --from-literal=mysql_port=3306 --from-literal=mysql_db_user=rajesh-db1
(or)

mysql-config.yaml
--------------------------------------------------
```
apiVersion: v1
kind: ConfigMap
metadata:
    name: mqsql-config
    namespace: default # configmaps are used with in the namespace.
data:
    mysql_port: 3306
    mysql_db_user: rajesh-db1
```
--------------------------------------------------

	$ kubectl create -f mysql-config.yaml
 
Step-2: Injecting complete configMap   

main.yaml
---------------------------------------------------
```
apiVersion: v1
kind: Pod
metadata: 
    name: nginx-pod
    labels:
        app: nginx
spec:
    containers:
    -   name: nginx
        image: nginx
        envFrom:    			# configmap is mapped to a specific contiainer.
        -   configMapRef:
            -   name: mysql-config  	# mapping complete configmap
```	
---------------------------------------------------

Scenario-2: Creating configMap and Injecting one variable from a configMap
---------------------------------------------------------------------------

Step-1: Creating a configMap:
myapp-color.yaml
---------------------------------------------------
```
apiVersion: v1
kind: ConfigMap
metadata:
    name: myapp-color
    namespace: default # configmaps are used with in the namespace.
data:
    APP_COLOR: RED 
    APP_USER: Abhilash
```
---------------------------------------------------
step-2: inject configMap:

```
spec:
  containers:
  - name: nginx
    image: nginx
    env:		# only one environment variable is mapping
    -  name: APP_COLOR
       valueFrom:
         configMapKeyRef:
            name: myapp-color
            key: APP_COLOR
```

Scenario-3: Creating a configMap from File and Injecting a file as configMap in main YAML file
-----------------------------------------------------------------------------------------------
app-env.properties
-----------------------------------------------
```
APP_COLOR: RED
APP_NAME: rajesh
APP_DB_NAME: DB1
APP_DB_USER: db-user1
```
------------------------------------------------
step-1: creating a configMap from a file app-env.properties

	$ kubectl create configmap my-config --from-file=app-env.properties

Step-2: injecting a file as a configMap

------------------------------------------------- 
```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: my-image:latest
    volumeMounts:
    - name: app-volume
      mountPath: /path/to/mount			#  where you would mount the volume inside your container.
      readOnly: true  				# Optionally, specify if the volume should be read-only
  volumes:
  - name: app-volume
    configMap:
      name: my-config
```
----------------------------------------------
	$ kubectl get configmaps 			--> to list the configmaps
 	$ kubectl describe configmaps my-config 	--> to view the configMap
	$ kubectl edit configmap configmap-name 	--> to edit or update the configmap
	$ kubectl delete configmap configmap-name 	--> to delete the configmap

----------------------------------------------------------
Secrets: Generic
----------------------------------------------------------
secrets are used to store the sensitive data like passwords and keys. so that they are encripted. key encripting format base64. 2 steps involved. Secrets are not encrypted, They only encoded. so it is not safer in that sense. However, some best practices around using secrets make it safer.

    1. create secrets
    2. inject it into pod

### step-1: creating secrets:
------------------------------
	$ kubectl create secret generic my-secret --from-literal=db_host=mysql --from-literal=DB_user=root --from-literal=DB_password=password
	(or)
	$ kubectl create secret generic my-secret --from-file=secrets.properties.

before creating the secret file, we need to convert values in to encripted format. below we use base64 to encripy our data.

encrypt
-----------------------------
	$ echo -n "root" |base64 
	cm9vdA==
	$ echo -n "password" |base64
	cGFzc3dvcmQ=
	$ echo -n "mqsql" |base64
	bXFzcWw=

decrypt
------------------------------

	$ echo -n "cm9vdA==" |base64 --decode 		--> to decript the value
	$ echo -n "cGFzc3dvcmQ=" |base64 --decode

-------------------------------
secret.yaml
--------------------------
```
apiVersion: v1
kind: Secret
metadata:
    name: my-secrets
data:
    DB_user: cm9vdA==
    DB_Password: cGFzc3dvcmQ=
    DB_Host: bXFzcWw=

 ```   
---------------------------

	$ kubectl create -f secret.yaml
	$ kubectl get secrets my-secret
	$ kubectl get secrets my-secret -o wide 	--> to show the output in yaml file
	$ kubectl describe secrets my-secret

step-2: injecting configMap in to the pod :
--------------------------------------------
```
apiVersion: v1
kind: Pod
metadata: 
    name: nginx-pod
    labels:
        app: nginx
spec:
    containers:
    -   name: nginx
        image: nginx
        envFrom:    # configmap is mapped to a specific contiainer.
        - secretRef:
          - name: my-secret  # mapping complete configmap
```	  
--------------------------------------------------------------------
Injecting configMap specific eliment in to the pod:
--------------------------------------------------------------------
```
spec:
    containers:
    -   name: nginx
        image: nginx
        env:    # configmap is mapped to a specific contiainer.
        - name: DB_Password
          valueFrom:
            secretKeyRef:
                name: my-secret  # secret configmap file
                key: DB_Password

```		
-----------------------------------------------------------------------------


Note: checking-in secret object definition files to source code repositories. Enabling Encryption at REST for Secrets so they are stored encrypted in ETCD. Also the way kubernetes handles secrets as below

	1. A secret is only sent to a node if a pod on that node requires it.
	2. Kubelet stores the secret into a tmpfs so that the secret is not written to disk storage.
	3. Once the Pod that depends on the secret is deleted, kubelet will delete its local copy of the secret data as well.

----------------------------------------------------------
Secrets: Encrypting Secret Data at Rest usting ETCD 
----------------------------------------------------------
We can encrypt secrets using ETCD. to encryption using ETCD, check `etcdctl` is available or not. otherwise install it.
	
	$ sudo apt-get install etcd-client	# this will install etcdctl
	$ etcdctl

step-1: check REST encryption is enabled or not
-------------------------------------------------
first thing to encrypt dats using ETCD, we need to check the REST encription is enabled or not in kube-api-server. The kube-apiserver process accepts an argument `--encryption-provider-config` that controls how API data is encrypted in etcd. to verify the option is enabled or not by looking in to kube-apiserver config.
	
	$ kubectl -n kube-system describe pod kube-apiserver-controlplane 	# to check teh encryption is enabled or not

Step-2: create configuration for REST encryption : enc.yaml
------------------------------------------------------------
```
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:		# choose which services need to be encrypted with etcd. like secrets/configmaps/other.
      - secrets
    providers:
      - aescbc:
          keys:
            - name: key1
              secret: <BASE 64 ENCODED SECRET>
      - identity: {}
```      
---------------------------------------------------------

	$ head -c 32 /dev/urandom | base64 	--> randam encryption and use it in config file.
	
to effect the above changes, we need to edit the `kube-apiserver` manifest file availabe in `/etc/kubernetes/manifest/` pod definition file and update the `enc.yaml` (REST encryption file) in the  `--encryption-provider-config=/etc/kubernetes/enc/enc.yaml` and update the `volumeMounts` and `volumes` and restart the kube-apiserver as below. for more details refer this link.

https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/

first create a secret
-----------------------

	$ kubectl create secret generic my-secret1 --from-literal mykey=mypassword1 
	$ kubectl describe secret my-secret1
	$ kubectl get secret my-secret1 -o yaml >my-secret1.yaml

to read the secret files, use the below command and check. 

	$ ETCDCTL_API=3 etcdctl \
   		--cacert=/etc/kubernetes/pki/etcd/ca.crt   \
   		--cert=/etc/kubernetes/pki/etcd/server.crt \
   		--key=/etc/kubernetes/pki/etcd/server.key  \
   		get /registry/secrets/default/secret1 | hexdump -C

Post configuring the encryption settings we can convert the un encrypted secret file using the below command with out changing the actual values. this below command replaces all the unencrypted secrets. 

 	$ kubectl get secrets --all-namespaces -o json | kubectl replace -f -
  
----------------------------------------------------------------------------------------
Sidecar Contaienrs : Multi-Container PODS
----------------------------------------------------------------------------------------
Sidecar container pods are run along side with main application container in a pod. they will stay alive as long as the main container is live. if one of the pods goes down or crashes, both containers will get restarted. both the containers are expected to stay alive at all times. Containers that are co-exist with one or more containers called multi container pods, this pods will get destroied and created together. they even use same space. both sidecar and muticontainer pods are same. 

some of the usecases of the sidecar containers are:

	1. logging sidecar
 	2. monitoring sidecar
  	3. security sidecar
   	4. network sidecar
    	5. database sidecar
     	6. encryption sidecar and etc. 

multi-pod.yaml
-----------------------------
```
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp
  labels:
    app: simple-webapp
spec:
  containers:
  - name: simple-webapp
    image: simple-webapp
    ports:
    - containerPort: 80
  - name: log-agent
    image: log-agent
```
-----------------------------

----------------------------------------------------------------------------------------
Init container:
----------------------------------------------------------------------------------------
InitContainers in Kubernetes are specialized containers that run before the main application containers in a Pod. They're primarily used to perform setup or initialization tasks required by the main application or containers within the Pod. InitContainers run in sequence, one after another, before the main containers start. Each InitContainer must complete successfully before the next one starts.  InitContainers help keep the setup logic separate from the application logic, ensuring that the main container starts only when prerequisites are met.

InitContainers are commonly used for actions like:

	1. Preparing the environment.
	2. Pulling necessary files or data.
	3. Running configuration scripts.
	4. Initializing volumes or network resources.

-------------------------------------------
```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox:1.28
    command: ['sh', '-c', 'until nslookup myservice; do echo waiting for myservice; sleep 2; done;']
  - name: init-mydb
    image: busybox:1.28
    command: ['sh', '-c', 'until nslookup mydb; do echo waiting for mydb; sleep 2; done;']
```
-----------------------------------------------------------------

----------------------------------------------------------------------------
Self Healing Applications:
----------------------------------------------------------------------------
Kubernetes supports self-healing applications through ReplicaSets and Replication Controllers. The replication controller helps in ensuring that a POD is re-created automatically when the application within the POD crashes. It helps in ensuring enough replicas of the application are running at all times.


Probes :
----------------------------------------------------------------------------
In Kubernetes, probes are used to check the health and status of containers. There are three main types of probes: readiness probes, liveness probes, and startup probes. Each serves a different purpose in maintaining the health and availability of applications running in a Kubernetes cluster.

	1. Readiness Probe: Purpose: Determines if a container is ready to start accepting traffic.
 	2. Liveness Probe: Purpose: Determines if a container is still running.
  	3. Startup Probe: Purpose: Determines if a container has started successfully.

Readiness Probe:
------------------
Purpose: Determines if a container is ready to start accepting traffic.
Use Case: Used to prevent traffic from being sent to a container that is not ready to handle requests. This is particularly useful during initial startup or when the container is undergoing maintenance or updates.
Example: If a web server needs some time to load its configuration files or warm up, the readiness probe can ensure that the container only starts receiving traffic once it is ready.

Liveness Probe:
------------------
Purpose: Determines if a container is still running.
Use Case: Used to restart a container if it is detected to be unhealthy or in a failed state. This helps in recovering from situations where the application is stuck or has crashed.
Example: If a container runs into a deadlock or an unhandled exception, the liveness probe can detect this and restart the container to restore normal operation.

Startup Probe:
-------------------
Purpose: Determines if a container has started successfully.
Use Case: Useful for applications that have a long startup time. The startup probe is used to give containers a chance to start up properly before the liveness and readiness probes are applied.
Example: For a complex application that requires a longer time to initialize, the startup probe ensures that Kubernetes gives it enough time to start without being prematurely restarted by a liveness probe.
	
Custom Probes:
-------------------
While the above three are the primary types, you can configure custom probes to suit specific needs using different handlers:

	HTTP Probes: 	Checks the response of an HTTP GET request.
	TCP Probes: 	Checks the TCP socket connection.
	Exec Probes: 	Runs a command inside the container to check its health
 
---------------------------------------------------------------------------
Readiness Probe:
---------------------------------------------------------------------------
Ensuring that a container is ready to serve traffic before it receives requests.  Implement a readinessProbe to confirm the container's readiness. Set it to validate the container's health before adding it to service endpoints.

-----------------------
 ```
containers:
- name: nginx
  image: nginx
  readinessProbe:
      httpGet:
        path: /healthz
        port: 8080
      initialDelaySeconds: 3
      periodSeconds: 3
```
---------------------------------

Healthcheck (readiness):
-------------------------
Ping the app every 3 sec and  make sure it's healthy (ie. accepting HTTP requests). If fail two subsequent pings, cordone it off (prevents cascades). Must pass two subsequent health checks before can accept traffic again.

-------------------------
```
readinessProbe:
  successThreshold: 2
  failureThreshold: 2
  periodSeconds: 10
  timeoutSeconds: 5
  httpGet:
    path: /management/health
    port: web-traffic
```
-------------------------

---------------------------------------------------------------------------------------------------------
Liveness Probe:
--------------------------------------------------------------------------------------------------------
Continuous monitoring to ensure that a container is running without issues. a livenessProbe to periodically check the container's health. Use it to restart the container if it becomes unresponsive or encounters issues.

-------------------------
```
 livenessProbe:
      httpGet:
        path: /healthz
        port: 8080
      initialDelaySeconds: 3
      periodSeconds: 3
```
----------------------------
App has Died (liveliness):
--------------------------
If app fails 3 consecutive health checks, 30 seconds apart, reboot the container. Maybe app got into an unrecoverable state like Java ran out of heap memory.

-------------------------
```
livenessProbe:
  successThreshold: 1
  failureThreshold: 3
  periodSeconds: 30
  timeoutSeconds: 5
  httpGet:
    path: /management/health
    port: web-traffic
```
-------------------------
  
---------------------------------------------------------------------------------------------------------
Startup Probe:
---------------------------------------------------------------------------------------------------------
For applications that have a longer initialization time or complex startup procedures. It is used to indicate if the application inside the Container has started. If a startup probe is provided, all other probes are disabled. In the given example, if the request fails, it will restart the container. Once the startup probe has succeeded once, the liveness probe takes over to provide a fast response to container deadlocks. If not provided the default state is Success.

-------------------------
```
startupProbe:
      httpGet:
        path: /healthz
        port: 8080
      initialDelaySeconds: 3
      periodSeconds: 3
```
------------------------------

App Initialization (startup):
-------------------------------
Spring app that is slow to start - anywhere between 30-120 seconds. Don't want other probes to run until app is started. Check it every 10 seconds (give up after 5 sec) for up to 180s before k8s gets into a crash loop.

-------------------------
```
startupProbe:
  successThreshold: 1
  failureThreshold: 18
  periodSeconds: 10
  timeoutSeconds: 5
  httpGet:
    path: /management/health
    port: web-traffic
```
-------------------------

----------------------------------------------------------------------------------------
OS Upgrade :
----------------------------------------------------------------------------------------
before you proced to deploy any upgrades, we must empty the node then apply the upgrades. 
	
	$ kubectl drain node01		--> i will move all k8s service to other nodes in cluster
	$ kubectl uncordon node01  	--> post paching, we need to remove drain tag using this command. so its starts taking tasks
	$ kubectl cordon node01 	--> just to make it non schedulable node

-----------------------------------------------------------
Kubernetes Software vesrions:
-----------------------------------------------------------
kubernetes API versions, software releases are of 3 parts. example: v1.11.3, v1(major).11(minor).3(patch). this is the k8s versions are released. there are also releases like alpha & beta versions. 

--------------------------------------------
kubenetes Cluster upgrade strategies: 
--------------------------------------------
kube-apiserver is the primary component that communicate with others resources, so the versions should folow as follows. k8s supports only the latest 3 versions. if we upgrade from v1.10 to v1.13, the recommunded approch is to upgrade 1 by 1 higher versions. means we need to upgrade frist from v1.10 to v1.11 then v1.11 to v1.12 then v1.12 to v1.13.

	Software 		Versions supports
	-----------------------------------------------
	kube-apiserver		v1.10
	control-manager		v1.9 | v1.10
	kube-scheduler		v1.9 | v1.10
	kubelet			v1.8 | v1.9 | v1.10
	kubeproxy		v1.8 | v1.9 | v1.10 
	kubectl			v1.9 | v1.10| v1.11
	ETCD cluster
	core DNS

Kubernetes cluster upgrade using `kubeadm` as production setup
----------------------------------------------------------------
upgrade can be done in 2 steps, 1st upgrade master node, then upgrade the worker nodes. the master going down doen't effect the running application running in workernode, but the k8s services are not accessable as we bringdown the k8s api-server & schedulesrs. 

	strategy-1: bring down all with downtime, and bringup all post upgrade. 
	strategy-2: bringing down 1 by 1 and bringing up parallel, it doesn't require downtime
	strategy-3: adding new nodes to the cluster with new softwares and move the objects to new node and decomission the old one.

kubeadm - upgrade
------------------------------
kubeadm will upgarde the k8s componets, but we must upgrade manually for `kubelet` as `kubeadm` will not do that.

	$ kubeadm upgrade plan
	$ kubeadm version

Note: first we need to upgrade `kubeadm`, 

	$ kubectl drain controlplane
	$ sudo apt-get upgrade -y kubeadm=1.12.0-00 --> upgrade the `kubeadm`
	$ kubeadm upgrade plan
	$ kubeadm upgrade apply v1.12.0	
	$ kubeadm get nodes 

still teh versions details are not reflected as `kubelet` is still not upgraded.

	$ sudo apt-get upgrade -y kubelet=1.12.0-00
	$ systemctl restart kubelet
	$ kubectl get nodes 			--> there you can see the master node got upgraded. 

Upgrading the K8s cluster using `kubeadm`: Ubuntu 
----------------------------------------------------

step-1: determine which version to upgrade
------------------------------------------
	
	$ apt update
	$ apt-cache madison kubeadm 

step-2: Upgrading control plane nodes
--------------------------------------
The upgrade procedure on control plane nodes should be executed one node at a time. Pick a control plane node that you wish to upgrade first. It must have the `/etc/kubernetes/admin.conf` file
	
	$ apt-mark unhold kubeadm && \
 	  apt-get update && apt-get install -y kubeadm=1.26.0-00 && \
 	  apt-mark hold kubeadm
	$ kubeadm version
	$ kubeadm upgrade plan
	
Note: kubeadm upgrade also automatically renews the certificates that it manages on this node. To opt-out of certificate renewal the flag --certificate-renewal=false can be used. For more information see the certificate management guide.	

Note: If kubeadm upgrade plan shows any component configs that require manual upgrade, users must provide a config file with replacement configs to kubeadm upgrade apply via the --config command line flag. Failing to do so will cause kubeadm upgrade apply to exit with an error and not perform an upgrade.
	
	$ sudo kubeadm upgrade apply v1.26.0-00	--> successful completion of 1st master use same  steps for other masters.
	$ sudo kubeadm upgrade node --> instead apply use node

step-3: upgrade the `kubelet` `kubectl` component
--------------------------------------------------
before 	upgrading the `kubelet` drain the controlplane node and install the `kubelet`
	
	$ kubectl drain controlplane --ignore-daemonsets
	$ apt-mark unhold kubelet kubectl && \
	  apt-get update && apt-get install -y kubelet=1.26.0-00 kubectl=1.26.0-00 && \
	  apt-mark hold kubelet kubectl
	$ sudo systemctl daemon-reload
	$ sudo systemctl restart kubelet
	$ sudo uncordon controlplane
	$ kubectl get nodes	--> check the k8s version 

step-4: Upgrade worker nodes 
-----------------------------------------
to upgrade the worker node it should done 1 worker node at a time. ssh to the worker node and run the commands in node01 host.

upgrade `kubeadm` in worker node

	$ apt-mark unhold kubeadm && \
	  apt-get update && apt-get install -y kubeadm=1.26.0-00 && \
	  apt-mark hold kubeadm

upgrade the `kubelet` configuration

	$ sudo kubeadm upgrade node

drain the worker node node01

	$ kubectl drain node01 --ignore-daemonsets

upgrade `kubelet` and `kubectl` commands

	$ apt-mark unhold kubelet kubectl && \
	  apt-get update && apt-get install -y kubelet=1.26.0-00 kubectl=1.26.0-00 && \
	  apt-mark hold kubelet kubectl
	
restart the `kubelet` services	

	$ sudo systemctl daemon-reload
	$ sudo systemctl restart kubelet
	
uncordon the worker node, so it start scheduing the new nodes

	$ kubectl uncordon node01
	
	$ kubeadm token list	
	$ systemctl status kubelet --> `kubelet` service status check 
	$ journalctl -xeu kubelet --> view the service logs with 

------------------------------------------------------------------------------------------
Backup and Restore Methods: 
------------------------------------------------------------------------------------------
In k8s what components are required to take backups, 

	1. Resource Configurations 
	2. ETCD
	3. Persistant volumes.
	
saving the all resources and all objects in a cluster backup
   
	$ kubectl get all --all-namespaces -o yaml >all-deploy-service.yaml

while configuring the ETCD, we have defined the data storage location as `--data-dir=/var/lib/etcd`. this need to take backup. using the below command. 	
	
	$ ETCDCTL_API=3 etcdctl snapshot save snapshot.db
	$ ETCDCTL_API=3 etcdctl snapshot status snapshot.db
	
to restore the backup, first need to stop the kube-api-server 

	$ service kube-apiserver stop 
	$ ETCDCTL_API=3 etcdctl snapshot restore --data-dir /var/lib/etcd-from-backup/snapshot.db
	
edit the etcd.service file with new `--data-dir` path and restart the service

	$ systemctl daemon-reload
	$ service etcd restarted
	
Note: every time you use the etcd command we have to pass the server.crt, ca.crt, key file and endpoint url, then only it will work. 

	$ ETCDCTL_API=3 etcdctl snapshot save snapshot.db \
	  --endpoints=https://10.1.64.10:2379 \
	  --cert=/etc/kubernetes/pki/etcd/server.crt \
	  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
	  --key=/etc/kubernetes/pki/etcd/server.key \
	  
---------------------------------------------------------	  
working with ETCDCTL:
---------------------------------------------------------
`etcdctl` is a command line client for etcd. the ETCD key-value database is deployed as a static pod on the master. The version used is v3. To make use of `etcdctl` for tasks such as back up and restore, make sure that you set the ETCDCTL_API to 3. You can do this by exporting the variable `ETCDCTL_API` prior to using the etcdctl client. 
 
This can be done as follows: $ export ETCDCTL_API=3	

	$ etcdctl snapshot save -h 	--> and keep a note of the mandatory global options.Since our ETCD database is TLS-Enabled, the following options are mandatory:
	–cacert               	 	verify certificates of TLS-enabled secure servers using this CA bundle
	–cert                    	identify secure client using this TLS certificate file
	–endpoints=[127.0.0.1:2379] 	This is the default as ETCD is running on master node and exposed on localhost 2379.
	–key                  		identify secure client using this TLS key file

Example: Backup and restore:
----------------------------
	$ kubectl config
	$ kubectl config get-clusters		--> list the clusters in k8s
	$ kubectl config use-context cluster1 	--> switch between clusters.
	$ kubectl get nodes
	
--------------------------------------------------------------------------------------
kubernetes Security:
--------------------------------------------------------------------------------------
1. password based authentication disabled
2. SSH key based authentication enabled

Who can access the K8s cluster?
	2. Files
	3. cetificates
	4. external authentication providers - LDAP
	5. service accounts

What they can do?
	1. RBAC authorization
	2. ABAC authorization
	3. Node authorization
	4. Webhook mode

all components in cluster such as api-server, etcd, kubelet, kube-controller, kube-scheduler are communicated with certificates. 

![image](https://user-images.githubusercontent.com/33980623/230711667-11f27cee-50c2-42e6-8ae1-4951b7c74fd1.png)

Note: all user access are managed by kube-apiserver. 

Authentication mechanisum in K8s:
----------------------------------
Authentication mechanisum can be achived in different ways, 
	1. Static password file
	2. staic token file
	3. certificates
	4. identification services (third party)
	
View certificate details:
-------------------------
	$ openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text 
	$ journalctl -u etcd.service -l --> use this to inspect the logs
	
Note: if the kube-apiserver is down then, check the `docker ps -a` and `docker logs id`

---------------------------------------------------------------------------------
kubeconfig:
---------------------------------------------------------------------------------
the kubeconfig file resides on $HOME/.kube/config. this file will tell you which user is authorized to connect which namespace. this is how the kubeconfig file works.

	$ kubectl config view
	$ kubectl config use-context
	$ kubectl config --kubeconfig=/root/my-kube-config use-context research
	$ kubectl config --kubeconfig=/root/my-kube-config current-context

-----------------------------------------------------------------------------------
Authorization: --authorization-mode=Node,RBAC,Webhook
-----------------------------------------------------------------------------------
there are various types

	1. Node 
	2. ABAC (Atribute based authorization )
	3. RBAC
	4. Webhooks

RBAC: Role-based access control (RBAC) is a method of regulating access to computer or network resources based on the roles. To enable RBAC, start the API server with the `--authorization-mode` flag set to a comma-separated list that includes RBAC.

Webhook: to outsource the authorization we use webhooks. the third-party tools like `Open-policy-agent`. 

------------------------------------------------------------------------------------
RBAC: Role based access control
------------------------------------------------------------------------------------
To create roles and assing the roles, we need to create role based YAML file. then binding the user to that role using role-binding YAML file. 

Roles & RoleBindings
--------------------------
Roles and RoleBindings: Roles and RoleBindings are limited to a particular namespace within a Kubernetes cluster. They define permissions within that namespace only.
Usage: Roles are used to define a set of permissions (e.g., to access resources like Pods, Services) within a specific namespace. RoleBindings are used to bind these roles to users, groups, or service accounts within the same namespace.
	
	$ kubectl create role developer --resource pods --verb=list,create,delete		--> developer role is created
	$ kubectl create rolebinding dev-user-binding --user dev-user --role developer 		--> dev-user added to developer role

developer-role.yaml
--------------------------------------
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
	name: developer		# creating "developer" role
rules:
	- apiGroups: [""]
	  resources: ["pods"]
	  verbs: ["list", "get", "create", "update"]
	- apiGroups: [""]
	  resources: ["ConfigMap"]
	  verbs: ["create"]
```
---------------------------------------

devuser1-developer-binding.yaml
-----------------------------------
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
	name: devuser-developer-binding
subjects:
- 	kind: User
	name: dev-user
	apiGroup: rbac.authorization.k8s.io
roleRef:
	kind: Role
	name: developer
	apiGroup: rbac.authorization.k8s.io
```
---------------------------------

	$ kubectl create -f developer-role.yaml
	$ kubectl create -f devuser1-developer-binding.yaml 
	
	$ kubectl get roles
	$ kubectl get rolebindings
	
	$ kubectl describe role developer

Q: how to check what access i have?
-------------------------------------------
	$ kubectl auth can-i create deployments

----------------------------------------------------------------------------------------------
Clusterrole & clusterrolebinding
----------------------------------------------------------------------------------------------
ClusterRoles and ClusterRoleBindings are applied cluster-wide. ClusterRoles are used to define a set of permissions that apply across the entire cluster (e.g., to access nodes, namespaces, PersistentVolumes). ClusterRoleBindings are used to bind these cluster-wide roles to users, groups, or service accounts across the entire cluster.

	$ kubectl create clusterrole storage-admin --resource persistentvolumes --resource storageclasses --verb get,watch,list,create,delete
	$ kubectl create clusterrolebinding michelle-storage-admin --clusterrole storage-admin --user michelle
	$ kubectl edit clusterrolebindings.rbac.authorization.k8s.io storage-admin
   	$ kubectl edit clusterrole storage-admin 
        $ kubectl edit clusterrolebindings.rbac.authorization.k8s.io michelle-storage-admin 

------------------------------------------------------------------------------------------------
ServiceAccounts:
------------------------------------------------------------------------------------------------
serviceaccounts are used to provide access to the applications that required permission on k8s to work.

service accounts are of 2 types, 
	1. useraccount (admins accessing the k8sclusters admin activities (or) developer accessing k8s for developer activity)
	2. service account (applications like (promitheous/jenkins/etc) access to k8s given using service accounts)
	
	$ kubectl create serviceaccount account-1
	$ kubectl get serviceaccount
	$ kubectl descrbe serviceaccount account-1
	$ kubectl create token dashboard-sa		--> create token for the service created. 
	$ kubectl describe secret token-name 		--> found in the account describe command

Note: while creating the service accounts a token must be created, this token is used for the extenal application authentication. this tokens are encrypted secrets. serviceaccount tokens can be used as voulme mounts, if the application is deployed on the k8s cluster. 

for every namespace a default serviceaccount is created. this can be viewed from pod describe.

------------------------------------------------------------------------------------------------
imageSecurity:
------------------------------------------------------------------------------------------------

------------------------------------------------------------------------------------------------------
NetworkPolicies:
------------------------------------------------------------------------------------------------
Network traffic flow in 2 ways. 
	
	1. ingress Traffic
	2. egress traffic

Ingress Traffic: if the traffic comming into the application is called ingress-traffic
Egress Traffic: if the traffic going out  from the application is called egress-traffic.
 
Network polices: in the k8s by default all the PODS comminicates to all the PODS in the POD network. This is not ideal for critical application. so we need to restric the pods traffic it should communitcate with. thats where this network-policies are used. 

--------------------------------
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-db-ingress
  namespace: critical-space  # Add namespace if necessary
spec:
  podSelector:
    matchLabels:
      app: db-pod
  policyTypes:
    - Ingress
  ingress:
  - from:
    - ipBlock:
        cidr: 192.168.1.10/32
    - podSelector:
        matchLabels:
          app: app-pod
      namespaceSelector:
        matchLabels:
          app: prod
    ports:
    - protocol: TCP
      port: 3306
 
```
--------------------------------
Note: what we mention in `policyType` that trafic only restricted. rest are by default communicate with all pods in cluster. 

Note: NetworkPolicies are implimented how we have configured them. Solutions that support network policies like(Kube-router,calico,romana,Weave-net). Solutions that didnt'support networkpolicies(fa

Note: if we try to take backup of the db pod into external server which is not part of k8s cluster, then we use `ipBlock` to enable the trafic.

---------------------------------------------------------------------------------------------
Volumes:
---------------------------------------------------------------------------------------------
docker containers are transient in nature, that means once the job is done they are destroied. so that the data stored in the container is lost once destroied. to persist data by the container we attach volumes to it. 

---------------------------------------
```
apiVersion: v1
kind: Pod
metadata:
  name: random-number
spec:
  containers:
  - name: alpine
    image: alpinee
	command: ["/bin/sh","-c"]
	args: ["shuf -i 0-100 -n 1 >> /opt/number.out;"]
	volumeMounts:
	- mountPath: /opt
	  name: data-volume
  volumes:
  - name: data-volume
    hostPath:
	  path: /data
	  type: Directory
```
---------------------------------------
```
  volumes:
  - name: data-volume
  	awsElasticBlockStore:
		volumeID: volume-id
		fsType: ext4
```
--------------------------------------
```
apiVersion: v1
kind: Pod
metadata:
  name: webapp
spec:
  containers:
  - name: event-simulator
    image: kodekloud/event-simulator
    env:
    - name: LOG_HANDLERS
      value: file
    volumeMounts:
    - mountPath: /log
      name: log-volume

  volumes:
  - name: log-volume
    hostPath:
      # directory location on host
      path: /var/log/webapp
      # this field is optional
      type: Directory
```	  
-----------------------------------------------------------------------------------------------------------------
PersistantVolumes:
-----------------------------------------------------------------------------------------------------------------
A PersistentVolume (PV): is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes. It is a resource in the cluster just like a node is a cluster resource. 

---------------------------------------------------------
```
apiVersion: v1
kind: PersistantVolume
metadata:
  name: pv-vol1
spec:
 accessModes:
   - ReadWriteOnce
 capacity:
   storage: 1Gi
 awsElasticBlockStore:
   volumeID: volume-id
   fsType: ext4  
```
---------------------------------------------------------
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-log
spec:
  accessModes: 
  - ReadWriteMany
  capacity:
    storage: 100Mi
  hostPath:
    path: /pv/log
```	
--------------------------------------------------------
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: claim-log-1
spec:
  accessModes:
  - ReadWriteMany
  resources:
    requests:
	  storage: 50Mi
```	  
------------------------
 
  	$ kubectl create -f pv-definition.yaml

-------------------------------------------------------------------
PersistentVolumeClaim (PVC):
-------------------------------------------------------------------
is a request for storage by a user. It is similar to a Pod. Pods consume node resources and PVCs consume PV resources. Pods can request specific levels of resources (CPU and Memory). Claims can request specific size and access modes (e.g., they can be mounted ReadWriteOnce, ReadOnlyMany or ReadWriteMany,

------------------------------------
```
apiVersion: v1
kind: PersistantVolumeClaim
metadata:
  name: pv-vol1
spec:
 accessModes:
   - ReadWriteOnce
 resources:
   requests:
     storage: 500Mi
```
-----------------------------------

$ kubectl get persistantvolumes 
	 
----------------------------------------	 
```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: myclaim
```
----------------------------------------
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:alpine
    ports:
      containerPort: 80
    volumeMounts:
      - mountPath: /var/www/html
        name: local-pvc
  volumes:
  - persistentVolumeClaim:
      claimName: local-pvc
 ```     
------------------------------------------


	$ kubectl exec webapp -- cat /log/app.log

--------------------------------------------------------------------------------------------------------------
StorageClasses:
--------------------------------------------------------------------------------------------------------------
Storage classes are used to provision the PersitentVolumes dynamically. that means you don't have to manually create each time when the application require to map the PV to PVC volumes. the storage class dynamically creates PV volumes required for the POD according to the PVC claim resources properties defined. 

If no `reclaimPolicy` is specified when a StorageClass object is created, it will default to Delete.

Note: If you choose to use WaitForFirstConsumer, do not use nodeName in the Pod spec to specify node affinity. If nodeName is used in this case, the scheduler will be bypassed and PVC will remain in pending state. Instead, you can use node selector for hostname in this case as shown below.

---------------------------------
```
---
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: local-storageclass-1
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer

---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc-1
spec:
  accessModes:
	- ReadWriteOnce
  storageClassName: local-storageclass-1
  resources:
    requests:
	storage: 500Mi

---
apiVersion: v1
kind: Pod
metadata: 
	name: my-pod
spec:
  containers:
	- name: nginx
	  image: nginx
	  ports:
		- containerPort: 80
	  volumeMounts:
	  - mountPath:
		name: my-pod-v1
  volumes:
	- name: my-volume1
	  persistantVolumeClaim:
		claimName: my-pvc-1
```
---------------------------------
```
---
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: delayed-volume-sc
provisioner: kubernetes.io/no-provisioner	# local provisioning 
volumeBindingMode: WaitForFirstConsumer
...
------------------------
---
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: standard
provisioner: kubernetes.io/aws-ebs	# AWS provisioning
parameters:
  type: gp2
reclaimPolicy: Retain
allowVolumeExpansion: true
mountOptions:
  - debug
volumeBindingMode: Immediate
...

```
----------------------------------

-------------------------------------------------------------------------------------------------------------------
Networking Basics: Linux
-------------------------------------------------------------------------------------------------------------------
Q. What is Switching and Routing?

Switching: A network is an interface to connect one or more systems together. to connect one are more systems locally we use switches. a switch will create a network to connect both systems/computers together. systems that are in same network can only reach each other and ping each other.  `/etc/network/interfaces` file for persist the changes. 

	$ ip link
	$ ip addr
	$ ip addr add 192.168.1.10/24 dev eth0

Routing: there are 2 different networks A & B like A=`192.168.1.0` & B=`192.168.2.0`. to connect both networks together we use Router. the devices in network must know what is the `gateway` to connect with in diff network. the gateways for network A is `192.168.1.1` and for network B is `192.168.2.1`. this `gateways` need to be configured accross all network devices.  

	$ route
	$ ip route add 192.168.2.0/24 via 192.168.1.1 --> in Network A devices
	$ ip route add 192.168.1.0/24 via 192.168.2.1 --> In Network B devices
	
Gateway: is a door to connect with other network devices. for network A - 192.168.1.1 is the door. for network B - 192.168.2.1 is the door. 

Similerly if the network need to connect with Internet, need to add the internet ip `172.217.194.0/24` in all network devices. 
	
	$ ip route add 172.217.194.0/24 via 192.168.1.1 --> In network A devices
	$ ip route add 172.217.194.0/24 via 192.168.2.1 --> In network B devices
	
Q. What is the Default Gateway?

Default Gateway: Each device in the network configured with Gateway, so they know how to connect with other network devices. if you want the external network to connect your devices. then create a Default gateway which will connect to the all network using the gateway IP. you see 0.0.0.0 added in gateway that mean allow.

	$ ip route add default via 192.168.1.1 --> In network A
	$ ip route add default via 192.168.2.1 --> In network B

example: if there are 3 systems A-->B-->C. A connect B & B connect C. if A want to connect C, it should connect through B. we need to tell A connect via B to C, similerly tell C to connect via B to A.
	
	$ ip route add 192.168.2.0/24 via 192.168.1.6
	$ ip route add 192.168.1.0/24 via 192.168.2.6
	$ ping 192.168.2.5  --> still not able to send packages. 
	
Note: by default packets sends to other network devices are disabled in the Linux so we need to enable it via /etc/sysctl.conf.

	$ cat /proc/sys/net/ipv4/ip_forwarrd 	--> set to 1
	$ /etc/sysct.conf 			--> net.ipv4.ip_forward=1

--------------------------------------------------------------------------------------------
DNS: Domain Name Server
--------------------------------------------------------------------------------------------
Name Resolution: we give a name to host B=192.168.1.11 as DB. and provide a entry in the /etc/hosts file. you can name as you wish. its just a alias name for the ip. insted of using IP each time we can just use hostname. this can also done with DNS server entry for resolving the IP address with hostname. 
	
	$ ping 192.168.1.11
	$ ping db
	$ ping 

DNS server where the host DNS name entries are refered. insted of maintaining in /etc/hosts locally all organization servers look in the DNS server. so its like a central server where all servers lookup for the DNS names. to use the DNS server in your local system need to create a DNS-server entry inthe `/etc/resolv.conf` as below.

	$ cat /etc/resolv.conf	--> Nameserver/DNS server entry are available here.
	-----------------------
	nameserver	192.168.1.100	--> 192.168.1.100 is the DNS server 
	nameserver 	8.8.8.8
	search		mycompany.com 	prod.mycom
	-----------------------
	$ cat /etc/nsswitch.conf --> DNS name lookup order can be changed. 1st, 2nd where to lookup the entry
	
	$ ping mycompany.com
	
	$ nslookup www.google.com	--> nslookup Query only DNS server. not in the local entry in /etc/hosts file.
	$ dig www.google.com		--> similer to nslookup
	
nameserver that maintain by google. that has all names added is 8.8.8.8.  DNS server solutions out there, in this lecture we will focus on a particular one – CoreDNS.

--------------------------------------------------------------------------------------------------------
Network Namespaces:
--------------------------------------------------------------------------------------------------------



