# SFG Beer Works - Brewery Microservices

This project has a services of microservices for deployment via Docker Compose and Kubernetes.

You can access the API documentation [here](https://sfg-beer-works.github.io/brewery-api/#tag/Beer-Service)

## Connect with Spring Framework Guru
* Spring Framework Guru [Blog](https://springframework.guru/)
* Subscribe to Spring Framework Guru on [YouTube](https://www.youtube.com/channel/UCrXb8NaMPQCQkT8yMP_hSkw)
* Like Spring Framework Guru on [Facebook](https://www.facebook.com/springframeworkguru/)
* Follow Spring Framework Guru on [Twitter](https://twitter.com/spring_guru)
* Connect with John Thompson on [LinkedIn](http://www.linkedin.com/in/springguru)

## Running Services Via Docker Compose

```shell
docker compose -f compose-local.yaml up -d

docker compose -f compose-local.yaml down
```

## Infrastructure Services

### MySQL Service

```shell
kubectl create deployment mysql --image=mysql:5.7 --dry-run=client -o=yaml > mysql-deployment.yml
kubectl apply -f mysql-deployment.yml

kubectl create service clusterip mysql --tcp=3306:3306 --dry-run=client -o=yaml > mysql-service.yml
kubectl apply -f mysql-service.yml

kubectl get all
```

### JMS Service

```shell
kubectl create deployment jms --image=vromeo/activemq-artemis --dry-run=client -o=yaml > jms-deployment.yml
kubectl apply -f jms-deployment.yml

kubectl create service clusterip jms --tcp=8161:8161 --tcp=61616:61616 --dry-run=client -o=yaml > jms-service.yml
kubectl apply -f jms-service.yml

kubectl get all
```

### Inventory Service

```shell
kubectl create deployment inventory-service --image=springframeworkguru/kbe-brewery-inventory-service --dry-run=client -o=yaml > inventory-deployment.yml
kubectl apply -f inventory-deployment.yml

kubectl create service clusterip inventory-service --tcp=8082:8082 --dry-run=client -o=yaml > inventory-service.yml
kubectl apply -f inventory-service.yml

kubectl get all
```

### Inventory Failover Service

```shell
kubectl create deployment inventory-failover --image=springframeworkguru/kbe-brewery-inventory-failover --dry-run=client -o=yaml > inventory-failover-deployment.yml
kubectl apply -f inventory-failover-deployment.yml

kubectl create service clusterip inventory-failover --tcp=8083:8083 --dry-run=client -o=yaml > inventory-failover-service.yml
kubectl apply -f inventory-failover-service.yml

kubectl get all
```

### Beer Service

```shell
kubectl create deployment beer-service --image=springframeworkguru/kbe-brewery-beer-service --dry-run=client -o=yaml > beer-service-deployment.yml
kubectl apply -f beer-service-deployment.yml

kubectl create service clusterip beer-service --tcp=8080:8080 --dry-run=client -o=yaml > beer-service.yml
kubectl apply -f beer-service.yml

kubectl get all
```

### Order Service

```shell
kubectl create deployment order-service --image=springframeworkguru/kbe-brewery-order-service --dry-run=client -o=yaml > order-service-deployment.yml
kubectl apply -f order-service-deployment.yml

kubectl create service clusterip order-service --tcp=8081:8081 --dry-run=client -o=yaml > order-service.yml
kubectl apply -f order-service.yml

kubectl get all
```

### Readiness and Liveness Probe Configuration

```shell
kubectl apply -f inventory-deployment.yml
kubectl apply -f order-service-deployment.yml
kubectl apply -f beer-service-deployment.yml

kubectl get all
```

### Configure Graceful Shutdown

```shell
kubectl apply -f inventory-deployment.yml
kubectl apply -f order-service-deployment.yml
kubectl apply -f beer-service-deployment.yml

kubectl get all
```

### Spring Cloud Gateway Service

```shell
kubectl create deployment gateway --image=springframeworkguru/kbe-brewery-gateway --dry-run=client -o=yaml > gateway-deployment.yml
kubectl apply -f gateway-deployment.yml

kubectl create service nodeport gateway --tcp=9090:9090 --dry-run=client -o=yaml > gateway-service.yml
kubectl apply -f gateway-service.yml
```

### Deleting Services and Deployments

```shell
kubectl delete -f gateway-service.yml
kubectl delete -f gateway-deployment.yml
```

### Consolidated Logging with ELK Stack

#### Elasticsearch Configuration

```shell
kubectl create deployment elasticsearch --image=docker.elastic.co/elasticsearch/elasticsearch:7.12.1 --dry-run=client -o=yaml > elasticsearch-deployment.yml
kubectl apply -f elasticsearch-deployment.yml

kubectl create service clusterip elasticsearch --tcp=9200:9200 --dry-run=client -o=yaml > elasticsearch-service.yml
kubectl apply -f elasticsearch-service.yml
```

#### Kibana Configuration

```shell
kubectl create deployment kibana --image=docker.elastic.co/kibana/kibana:7.12.1 --dry-run=client -o=yaml > kibaja-deployment.yml
kubectl apply -f kibana-deployment.yml

kubectl create service nodeport kibana --tcp=5601:5601 --dry-run=client -o=yaml > kibana-service.yml
kubectl apply -f kibana-service.yml

kubectl delete -f kibana-service.yml
kubectl delete -f kibana-deployment.yml
kubectl apply -f kibana-deployment.yml
kubectl apply -f kibana-service.yml
```

#### Filebeat Configuration

```shell
kubectl apply -f filebeat-kubernetes.yaml
kubectl apply -f inventory-deployment.yml
kubectl apply -f order-service-deployment.yml
kubectl apply -f beer-service-deployment.yml

kubectl get services
```