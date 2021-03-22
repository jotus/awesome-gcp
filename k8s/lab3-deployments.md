
projectId:
ornate-entropy-307821

cluster:
app-cluster-1

zone:
europe-west3-a

gcloud container clusters get-credentials app-cluster-1 --zone=europe-west3-a
=============================================
- create deployment
- view traffic as we rollout
- observe deployment
- monitor and logging
- debugging


In GCP console:
1. create 3node cluster

2. auth cloud shell
> gcloud container clusters get-credentials <clustername> --zone=<zone name >

3. build docker image
- build blue ver
docker build -t myapp .
docker tag myapp gcr.io/ornate-entropy-307821/myapp:blue
docker push gcr.io/ornate-entropy-307821/myapp:blue

- build yellow
chnage colloro in python app and rebuild with different tag

docker build -t myapp .
docker tag myapp gcr.io/ornate-entropy-307821/myapp:yellow
docker push gcr.io/ornate-entropy-307821/myapp:yellow

- build broken pod - change title, and collor: darkred
docker build -t myapp .
docker tag myapp gcr.io/ornate-entropy-307821/myapp:bad
docker push gcr.io/ornate-entropy-307821/myapp:bad

Check in container registry new images

4. Create myapp-deployment, look into resources to chapter-2
 - update image names and tags
 > kubectl apply -f <file>
 > kubectl get pods
 > kubectl describe deployment <dep name>
 kubectl describe deployment deployment.apps/myapp-deployment

5. create service that:
-  match: myapp
- type LoadBalancer

> kctl apply -f <sercie yaml>
> kctl get services

Fetch ip addres on open browser


6. Simulate pod crash
kubectl delete pod <podname>


7. Simulate node crash
- get node name
>kubctl get pods -o=custom-columns=NODE:.spec.nodeName,NAME:.metadata.name

Tell scheduler not to use node
> kubectl taint nodes <node name>  -key=value:NoSchedule
Now remove pod from tainted node
> kubectl delete pod <podId>
Observe that pod was scheduled on differend node

- untake tainted node
> kubectl taint nodes <node name>  -key=value:NoSchedule-

8. Rollout bad version
- change image in deployment to "bad"
> kubectl apply -f app-deployment.yml --record

To view status:
> kubectl rollout status <deploymentName>
> kubectl rollout history <deploymentName>

See in browser new version

9. Rollback to previous version
> kubectl rollback undo <deployment revision>

observer pods
>kubectl get pods

10. Look at deployments in cloud console/workload tab
Revision section contains info about revision

11. Remove cluster