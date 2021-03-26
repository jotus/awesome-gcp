projectId:
ornate-entropy-307821

# Create blue green deployment

- create 3 node cluster
- auth  cloud shell
- create blue green deployment
- update apps version and switch traffic
- create canary deployment

- build image with version 1 (prepare files)
> gcloud builds submit --tag gcr.io/ornate-entropy-307821/myapp:v1

- craete blue deplo with v1 and apply
>kubectl apply -f  myapp-blue.yaml
> kubectl get pods

- create sevice with blue label - version: blue
myapp-service.yaml
```
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  selector:
    app: myapp
    version: blue
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8888
  type: LoadBalancer
````
>kubectl apply -f myapp-service.yaml
> kubectl get services
get ip and go to browser


- crate green deployment with version 2
> kubectl apply -f myapp-green.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-green
  labels:
    app: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
      version: green
  template:
    metadata:
      labels:
        app: myapp
        version: green
    spec:
      containers:
      - name: myapp
        image: gcr.io/ornate-entropy-307821/myapp:v2
        ports:
        - containerPort: 8888
```
Now should see 6 pods, traffic is flowing to v1 of app, since servcie is for v1 ( blue)

- update servcie to green deployment
> kubectl apply -f myapp-service.yaml
> kubectl describe service myapp-service // to see which servcie running

- prepare deployemnt for version 3 from blue deployment
 -- update image in deployment with tag v3
> kubectl apply -f myapp-blue.yaml
 -- swithc traffic to new deployment
 in myapp-service.yaml - version: blue
> kubectl apply -f myapp-service.yaml
> gcloud builds submit --tag gcr.io/ornate-entropy-307821/myapp:v4

# Canary deployment
- prepare deplyment with 1 replica, dont use additional label
- update service  and remove labelling: version: green, so traffic will be directed to all deployments
Idea is to have few deployments but with different container version and canary will have fewer replicas