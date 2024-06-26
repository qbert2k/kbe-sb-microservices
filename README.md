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