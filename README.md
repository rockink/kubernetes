# Kubernetes Practice 

## Building the project 
1. Create namespace `kubectl create -f namespaces.yml`. This will create `dev` and `logging` namespaces. 
`dev` is where we will operate our application. `logging` is where we will operate our logging infrastructure. 
2. Create products in `dev` namespace. `kubectl create -f products` 
3. Create logging infrastructure in `logging` namespace. 


### PRODUCT MICROSERVICE `./products`
Our first microservice in kubernetes is Product `./products/product.yml`. It is implemented using `Spring Boot`. It displays the laptop products in the JSON format. 

To run this, we would need to complete its dependencies, such as the database which it uses to store product information. **MongoDB** is the No-SQL solution for this. It is deployed by `mongo.yml`

## Access Product Service 
First we will need to expose the product service to the node. Since it is exposed by the NodePort, we would need to find the port that this service will expose to. 
1. Use `minikube ip` to find the ip address of the minikube. This will be ip address of our port. 
2. Use `kubectl -n dev get svc product -o wide` to find the external port. This is in range 3000. 
3. Use `curl` or `browser` to access the service.


### LOGGING 
This is a logging part of the kubernetes infrastructure. We design the microservice based infrastructure 
to scale as our application grows. And, with the growth of the application, we would have a system where 
the logging is scattered all over the microservices. This requires centralized logging. 

This stack uses ElasticSearch as database, Kibana as visualization tool and fluend as the logging agent 
deployed in a cluster. 

ElasticSearch is deployed as **StatefulSet** to leverage its stateful cluster. The order is maintained. 

Kibana is a stateless **Deployment** that can scale as it likes. 

Fluentd is a stateless **DaemonSet** that is deployed in each node, pulling out all the logs it can.

Accessing it locally:

1. Figure out the kibana pod first   `kubectl -n logging get pods | grep kibana`
2. Do a port forward: `kubectl port-forward ${KIBANA_POD} 5601:5601` 
3. Open a browser and see your logs.




