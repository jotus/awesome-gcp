https://github.com/ACloudGuru-Resources/CourseGKEBeginnerToPro

# Tagging image with full registry name
> docker tag myapp gcr.io/przechrzta/myapp

> docker push gcr.io/przechrzta/myapp
GO to gcp conteiner registry and find your image



# Manual setup of cluster
- setup credentials
> gcloud container clusters get-credentials you-cluster-1  --zone=us-central1-c
kctl get nodes
kctl get pods
kctl get services

- creating deployment
kctl create deployment nginx --image nginx
ktcl get pods
kctl expose nginx  --port=80  --type=LoadBalancer
kctl get services

# About pod
Logica unit that share network and storage configuration.
host one or more tightly coupled containers
eg. app and adapter to process output

dont bundle many apps into sigle pod

## Creating pod
> kubctl apply -f  mypod.yml


## Run mulit container pods
>kubectl apply -f multi.yml

# Attaching to pod in container
>kubectl exet -it <podname> -c <container name> -- /bin/bash
>kubectl exet -it mulit -c ftp-container -- /bin/bash
Ctrl-D to exit container and keep running

# ReplicaSet, 
ReplicaSet set of pod replicas
specified num of pods
ensures availability required numbers

# Deployments
- Object logically managing  pods and replicasets
- enforced by controller
- update, rollback, scale deployment
- scalling and error checking
- preffered obj. for deploying compute workloads



# Services
Assigns fixed ip to pod replicas.
exposes set of pods to network
 Types:
 - NodePort -  every node will get assigned port, advanced routing
 - ClusterIp - replicas not seen from outside, this is default service type
 - LoadBalancer - gcp network load balancer exposes  pods to world.
