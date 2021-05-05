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

# Monitoring,,logging, debugging
kctl get pods
kctl get events
kct get deployments
kctl describe deployment <deplName>


## Exposing 
kctl expose deploy nginx --port=80  --type=LoadBalancer
ktclt get services
//see in browser

## Getting logs - also visible in /workloads tab, dig into /pod/logs
>kctl get logs pod/<podname>  
or
>kubectl logs --selector app=nginx

## Monitoring
go to monitoring tab Monitoring/GKE Dashoboard

# Example -1
- create cluster with 1 node
- deploy nginx
>
-  Brake deployment - assuming 1 node,
kubectl scale --replicas=10 deployment nginx //this will fail when clusted has just one node

kubectl get pods
kubectl describe deploymen nginx
kubectl  get events //cluster event
// nothing is visible - no error
Look into workloads

try investigate pod
> kctl describe pod <podname> // should see error

# Example-2
- craete bad deployment
> kctl create deployment badluck1 --image=gcr.io/playgroudsds/myapp:v42
> kctl get pods
> kctl describe pod <podName>
> ktcl delete deployment <depl name>

# Example-3
- go to chapt-2/lecture-7
Breakd sth in requirements.txt
- build image
- tag image "broken
- docker push ...

- craete deployment
> kctl create deployment <name> --image=gcr.io/<imaage name>
> kctl get pods
> ktcl logs pod/<pod name>

- remove cluster

# ------------------------------------
# Deploying applications
- Example app
https://kubernetes.io/docs/tutorials/stateless-application/guestbook/

Look into lab 5

# About resources:
How interpret CPU in deployment.yaml
```
resources:
    requests:
    cpu: 100m // 0.1CPU
    memory: 100Mi
```
1CPU= 1000milicores
100m=1/10 CPU

When service is created it gets internal dns name that can be used by example. frontend pods to reference service

Exercise:
run lub-5

# Health checks
- check with http, tcp, or run command
- failed prob will replace pod
- built in monitoring
- checks done by kubelet

Example healthcheck
```
 containers:
      - name: myapp
        image: gcr.io/tim-acloud-guru/myapp:probes4
        ports:
        - containerPort: 8888
        livenessProbe:
          httpGet:
            path: /isalive
            port: 8888
          initialDelaySeconds: 3
          periodSeconds: 3
        readinessProbe:
          httpGet:
            path: /isready
            port: 8888
          initialDelaySeconds: 5
          periodSeconds: 5
```
## Readiness probe
- check done by kubelet
- define when pod ready to start serving
- buys time for pods to perform startup tasks
- traffic won't be directed to pod until it is ready
Probes done by handler:
- ExecAction
- TCPSocketAction
- HTTPGetAction
- initialDelaySeconds, periodSeconds, timeoutSeconds
- successThreshold, failureThreshold

# Pod states
Pending -> Running

Pending -> Running -> Succeeded //type of pod that runs action and exits

Pending -> Failed
Pending -> Unknown

# Accessign external services from inside POD
- using service endpoints - part of service discovery
- create Service object with no Selector
- kubernetes looks for an Endpoint with the same name
- Endpoint map external services to Service objects
- service discovery via internal DNS

``` no selector here
apiVersion: v1
kind: Service
  metadata: mongo
spec:
  type: ClusterIP
  ports:
  - port: 27017
    targetPort: 27017
```
and add Endpoint with the same name:
```
apiVersion: v1
kind: Endpoints
metadata:
 name: mongo
subnets:
  - addresses:
    - ip: 10.240.0.4
    ports:
    - port: 27017

```
when pod wants to access servcie it uses:
"mongodb://mongo.default.svc.cluster.local"  // this  is via dns name

## Services that map to external FQDNs, use externalName, no endpoint required
```
apiVersion: v1
kind: Service
  metadata: mongo
spec:
  type: ExternalName
  externalName: db12.mydb.com
```

## Can also access servcie via internal dns assigned to servcie and define endpoint that maps to physical compute instances

Endpoint maps to multiple ip addresses,
service will round-robin each IP

```
apiVersion: v1
kind: Endpoints
metadata:
  name: rabbitmq
subnets:
  - addresses:
    - ip: 10.245.0.1
    ports:
    - port: 31000
  - addresses:
    - ip: 10.245.0.2
    ports:
    - port: 31000

```

## Exposing using Sidecars
Sidecar provides connection to the external service
Container accesses servcie on localhost


# Volumes
# ------------------------------------------------------------------------
Volume Types:
 - emptyDir - deleted when pod removed from node, may be shared by multiple containers
 - gcePErsistentDisk - must be created beforehand, can be mounted by multiple  consumer-pods as ready-only
 - PersistentVolumeClaim - mount persistent volume into a pod, reuires matching PersistenVolume object.
PersistentVolume - piece of starge in cluster
 - manualy or dynamically provisioned
- configured with storageClass
- PersistenVolumeClaim is requset to consume PersistenVolume
- AccessModes defines how volume may be shared by multiple pods/consumers

Created Dysk is connected to PersistenVolume,
If you want to consume PersistenVolume need to create claim.

When you create peristent volume claim you choose access mode.

- ReadwriteOnce - single node can mount
- ReadOnlyMany - multiple node as ready only

Constraints
Deployments are designed for stateless appliacations
All replicas will share the same PersistenVolumeClaim
Volume mode must be ReadOnlyMany
Deployment strategy may cause volume deadlocks

Remarks:
When running deployment with persistent volume can only run single pod 


### Delete pod pro way - based on label
> kubectl delete pod -l app=mysql


### Cleanup - order
> kubectl delete -f service.yaml
> kubectl delete -f deployment.yaml
> kubectl delete -f  valumeClaim.yaml
> delete cluster

# --------------------------------------------------------------------
# ConfigMaps and secrets
## Secrets 
- injected during runtime
- consumed as env vars or volumes
- secrets are encoded only not encrypted by default
If U have access to cluster then also access to secret object.
May use Cloud KMS(key mgt Svc) or hashicorp Vault.

## ConfigMaps
Decouple configuration from docker image
Created from config files, dir with config files, literal key=val
- Values accessed as env vars
- may be mounted as Volumes

- to consume config map value define envVar and reference configMap.name, configMap.key
``` example
containers:
  -name: my-container
  image: nginx
  env:
  - name: CAR_NAME
    valueFrom: 
      configMapKeyRef:
        name: name1
        key: key1

```
- may also expose all key vals from configMap: reference confiMap.name
``` example - all keys
containers:
  -name: my-container
  image: nginx
  envFrom:
    - configMapRef:
      name: myconfig
````

- config map may store std config files eg. nginx configuration
  Config map mounted as Volume

  ```
containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: nginx-config
      mountPath: /etc/nginx
volumes:
  - name: nginx-config
    configMap:
      name: my-nginx-config
  ```



# Deployments patters - updating stateless application
## rolling updates - default update strategy
Controls how many pods may be created
Threshold for failed pods
Fully automatic

## canary deployments
Combines multiple deployments with single service
Testing with prod traffic

## blue-green deployments
Switch traffic from blue to green with sevice selector
All traffic immediately sent to new deployment.
Maintain two version of deployment

# To buid contaier, tag, and push to repository - container registry
> gcloud builds submit --tag gcp.io/wprzechrzta/myapp:v1

Create two services blue and green 
- deploy first 
- deploy second
docker inspect service <service name >


# Autoscaling
# ------------------------------------------------

## Horizontall scale Pods to increase capacity
 HorizontalPodAutoscaler

```
apiVersion: autoscaling/v1
kind: HorizontalPodAutoScaler
metadata:
  name: nginx-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx
  minReplicas: 3
  maxReplicas: 6
  targetCPUUtilizationPercentage: 60
```

## VErtically - increse resources assigned to pods
VErticalPodAutoscaller
automatically recommends or applies CPU and RAM requests for pods
more suited for stateful deployments
cannot worg with horizontal pod autoscaler

## Cluster Autoscaler , adds new VM, scale cluster nodes when demands is high, extra VM are required
 Node-pool cluster autoscaling in GKE
- nodes are added when pods dont have enough capacity to run
- underutilized nodes automatically deleted
- works best with node pools
- group VM nodes of specific type together, eg. high CPU, high RAM

## Best Practices
- CI/CD  on signle fixed node pool
- web frontedn workloads on generic autoscaling node pool
- RAM intensicve java apps in high RAM instance type node pool

## Lab-8 - to exercise scaling

Remarks
When U edig node pool settings it will destroy and recreate your deployment, so make sure that workload is distributed.
Edit node pool settings {click cluster tab, nodepools, and adjust limits}


# --------------------------------------------------------------------------------------------
# Helm
## Repository: 
https://artifacthub.io/

- look for Artifact Hub for charts

- install repo with desired manifests with name: bitnami
helm repo add bitnami http://charts.bitnami.com/bitnami

- example install:
repo: bitnami
> helm install my-wordpress  bitnami/wordpress
helm applies templated manifest to cluster from your charts.

Kubernetes yaml manifests implemented as go templates, allows dynamic use of configuration variables.

> helm install  my-wordpress bitnami/wordpress -f values.yaml
> helm status my-wordpress

## Demo installing elastic search
> helm repo add elastic https://helm.elastic.co

## Install elastic - config default values in repo
> helm install my-elasticsearch elastic/elasticsearch --version 7.12.0
>kubectl  get deployments
>kubectl get statefulsets
>kubectl get pods 
>kubectl get services
>kubectl get pv //persisten disks

> helm status my-elasticsearch
> helm test my-elasticsearch

## Changing replicaset:
relase name is: my-elasticsearch
>helm upgrade --set replicas=2 my-elasticsearch elastic/elasticsearch
Revision should be 2. 

>helm list
>helm delete my-elasticsearch
> kubectl get pods
>kubectl get pvc //pesisten volumes claims
> kubectl delete pvc <pvc name>

## Install with custom params
> helm install my-elasticsearch elastic/elasticsearch --version 7.12.0 -f values.yaml
> kubectl get pods --namespace=default -l app=elasticsearch-master -w
> kubectl get pv //this should show modifid persistent disk size eg. 50gi

## Useful helm command
> helm get manifest my-elasticsearch



# StatefulSets
Stateful set maintains identity for each deployed pod
manages pods based on a container spec, much like a Deployment.
When pod is rescheduled identity persists,
Guaratees the ordering and uniquensess of Pods

Stable network identity and persistent storage.
ordered graceful deployments scaling and updates.

StatefulSet is workload object but inside U define logical network name via service element. eg. service: pgsql
So each logical stateful node will get network name

When pods are created logical names are assigned.
GKE provides store volume for statefull set which is claimed by pgsql pods
Each new pod will get its own persistent volume.
When pod is deleted persisten volume is not and has to be removed manually.

## Pod management policies
- ordered Pod  management is default
- parallel Pod management is an option


## Update strategies
- Rolling updates delete and recreate Pods in descending order
- The scheduler waits for a Pod to be running and ready before proceeding
- updates can also be partitioned

# Finite tasks and init containers

# Jobs and CronJobs
- workload object that represents a finite task ana manages it to completion\
- non-parallel job sarts one pod and is complete when the pod terminates successfully

# Init Containers
- part fo the container array in a pod spec
- execute before app or other containers in a pod
- Init containers run in order and to completion, like logical sequence of events
This way U can separate startup code from application image.


Example bash loop:

sh -c
```
until nslookup mysql; do echo waiting for mysql; sleep 2; done;
```

# Tricks - Query DNS using container
Outpu: Record A

```
kubectl run dig --image=tutum/dnsutils --restar=Never --rm=true --tty --stdin --command -- dig <hostname>
``
