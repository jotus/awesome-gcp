

1. Create clusted and deploy nginx
> kctl create deployment nginx --image=nginx
> kubectl expose deploy nginx --port=80  --type=LoadBalancer
ktclt get services
//see in browser

2. Scale 

kubectl get pods
kubectl describe deploymen nginx
kubectl  get events 

//this will fail when clusted has just one node
kubectl scale --replicas=10 deployment nginx 


// nothing is visible - no error
//Look into workloads