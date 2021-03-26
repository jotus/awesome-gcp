
Example scaling app

- create 3 node cluster

- create deployment
> kubectl apply -f myapp-scaling.yaml

- create servcie
>kubectl apply -f myapp-service.yaml

- apply scaling 
> kubectl apply -f myapp-hpa.yaml

- open new terminal
watch kubectl get pods -o wide

- open 3rd terminal
>watch kubectl get nodes

- generate traffic to nodes using "hey" go tool
go get -u github.com/rakyll/hey

- check service
> kubectl get services

>hey <url to your site> // will run 200 requests

- run 25 concurrent requests for 2 minutes
> hey -c 25 -z 2m http://<ip>

Observe pod in other console and enjoy!!  scaling behaviour

