

- define ssd disks
```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast
provisioner: kubernetes.io/gce-pd
parameters:
  type: pd-ssd
```

> kubectl apply -f ssd-class.yaml
> kubectl get pods
> kubectl get storageclasses

- define headless service, that match app: cassandra, 
 ClusterIP: None means that query dns will return DNS A record with IP addresses of each pod, tha
```
apiVersion: v1
kind: Service
metadata:
  name: cassandra
  labels:
    app: cassandra
spec:
  clusterIP: None
  ports:
  - port: 9042
  selector:
    app: cassandra
```

> kubectl apply -f casasndra-servcie.yaml
> kubectl get pods
> kubectl get storageclasses
> kubectl get services

# Tricks - to see all resources tha we may query
> kubectl api-resources


## Loosing one node from cassandra
> kubectl get pods -o wide

Aks GKE not schedule any pod to to node
> kubectl cordon <nodename from prev command>
> kubectl delete pod cassandra-1