# Kubernetes Practice 

### PRODUCT MICROSERVICE `./products`
Our first microservice in kubernetes is Product `./products/product.yml`. It is implemented using `Spring Boot`. It displays the laptop products in the JSON format. 

To run this, we would need to complete its dependencies, such as the database which it uses to store product information. **MongoDB** is the No-SQL solution for this. It is deployed by `mongo.yml`


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




