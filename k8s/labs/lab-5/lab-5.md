

# Deploying applications
- Example app
https://kubernetes.io/docs/tutorials/stateless-application/guestbook/

- cfreate 3 node cluster
- open cloud shell
>mkdir lab5

- create redis master: deployment and service
kubectl apply -f redis-master-deployment.yml redis-master-service.yml

- create resdis slave deployment
> kubectl apply -f redis-slave-deployment.yml redis-slave-service.yml

> kubectl get services

- create frontend
Here forntend service uses LoadBalancer type of service to communicate with external world
Frontend service communicates with backend via internal dns service
env:
        - name: GET_HOSTS_FROM
          value: dns
> kubectl apply -f frontend-deployment.yml
> kubectl apply -f frontend-service.yml

- cleanup
remove all clusters
