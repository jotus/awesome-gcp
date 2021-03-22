
https://github.com/ACloudGuru-Resources/Course_GKE_Beginner_To_Pro/tree/master/Chapter_Two/Lecture_4_Lab

``` pod.yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  -name nginx
   image: nginx
   ports:
   -name: web
     containerPort: 80
```

## Create pod
kubectl apply -f pod.yml
kubectl get pods

It is possible to to port forwarding from pod to client shell
> kubectl port-forward nginx 8080:80
Run preview - upper right corner in gcp console

## Delete pod
kubectl delete -f pod.yml

## Run mulit container pods

>kubectl apply -f multi.yml

Attaching to pod in container
>kubectl exet -it <podname> -c <container name> -- /bin/bash
>kubectl exet -it mulit -c ftp-container -- /bin/bash

Try to login to server
FromClient:
ftp localhost
admin:password


# ---------------------------------------