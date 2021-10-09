## Introduction
Kubernetes Operators are software extensions to Kubernetes that provide custom resources for management of applications, services and their components.
<li>RabbitMQ Cluster Kubernetes Operator automates provisioning, management, and operations of RabbitMQ clusters running on Kubernetes.
<li>RabbitMQ Cluster Kubernetes Operator provides a consistent and easy way to deploy RabbitMQ clusters to Kubernetes and run them, including "day two" (continuous) operations. 
<li>RabbitMQ clusters deployed using the Operator can be used by applications running on Kubernetes or outside of Kubernetes.

## Setup 
<li> Download and install rabbitmq K8s cluster operator

```
kubectl apply -f https://github.com/rabbitmq/cluster-operator/releases/latest/download/cluster-operator.yml
```

<li> Check the rabbitmq namespace 

```
kubectl get all -o wide -n rabbitmq-system
```

<li> Confirm Service Availability

```
kubectl get customresourcedefinitions.apiextensions.k8s.io
```

<li> Deploy hello-world example

```
kubectl apply -f https://raw.githubusercontent.com/rabbitmq/cluster-operator/main/docs/examples/hello-world/rabbitmq.yaml
```

This will deploy in current namespace (default)

<li> Check hello-world cluster

```
kubectl get all -o wide -n default
NAME                       READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
pod/hello-world-server-0   1/1     Running   0          81s   172.17.0.7   minikube   <none>           <none>

NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                        AGE   SELECTOR
service/hello-world         ClusterIP   10.106.253.92   <none>        5672/TCP,15672/TCP,15692/TCP   82s   app.kubernetes.io/name=hello-world
service/hello-world-nodes   ClusterIP   None            <none>        4369/TCP,25672/TCP             82s   app.kubernetes.io/name=hello-world
service/kubernetes          ClusterIP   10.96.0.1       <none>        443/TCP                        89d   <none>

NAME                                  READY   AGE   CONTAINERS   IMAGES
statefulset.apps/hello-world-server   1/1     81s   rabbitmq     rabbitmq:3.8.21-management

NAME                                       ALLREPLICASREADY   RECONCILESUCCESS   AGE
rabbitmqcluster.rabbitmq.com/hello-world   True               True               82s
```

<li> Check cluster

```
C:\Users\91889>kubectl get customresourcedefinitions.apiextensions.k8s.io
NAME                            CREATED AT
rabbitmqclusters.rabbitmq.com   2021-10-06T11:08:32Z

C:\Users\91889>kubectl get rabbitmqclusters.rabbitmq.com
NAME          ALLREPLICASREADY   RECONCILESUCCESS   AGE
hello-world   True               True               4m16s
```

<li> To access rabbitmq management

```
kubectl port-forward "service/hello-world" 15672

Get username: kubectl get secret hello-world-default-user -o jsonpath='{.data.username}'
Get password: kubectl get secret hello-world-default-user -o jsonpath='{.data.password}'

```
URL: localhost:15672

```
Get clusterIp: kubectl get service hello-world -o jsonpath='{.spec.clusterIP}'
```

<li> To test the broker

```
kubectl run perf-test --image=pivotalrabbitmq/perf-test -- --uri amqp://uname:pwd@clusterIp
```

<li> Cleanup

```
kubectl delete pod perf-test
kubectl delete rabbitmqclusters.rabbitmq.com hello-world
```

## Test client
Login to RabbitMQ management portal and create a new user called "guest".

<li> Set the following in appplication.properties

```
spring.profiles.active=usage
test.client.duration=10000
spring.rabbitmq.host=localhost
spring.rabbitmq.port=5672
spring.rabbitmq.username=guest
spring.rabbitmq.password=guest
```
  
<li> Build the jar using
  
```
  mvn clean package
```
   
<li> To run sender, run
    
  ```
  java -jar rabbitmq-client-0.0.1-SNAPSHOT.jar --spring.profiles.active=test,sender
  ```
    
<li> Open another terminal and run reciever
    
  ```
    java -jar rabbitmq-client-0.0.1-SNAPSHOT.jar --spring.profiles.active=test,reciever
  ```
    
  
## References
This guide was created using [RabbitMQ tutorials](https://www.rabbitmq.com/kubernetes/operator/operator-overview.html)
  
