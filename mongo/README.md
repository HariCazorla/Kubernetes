# Kubernetes-basics
Using minikube to explore K8s concepts.
1. Create a mongodb deployment and read env variables from secret.
2. Create a mongodb service.
3. Create a mongo-express deployment and read mongodb url from configmap.
4. Create a mongo-express service as Loadbalancer.
5. Assign external IP address to the service.

## kubectl commands
#### Create deployment: 
```kubectl create deployment [name]```

#### Edit deployment: 
```kubectl edit deployment [name]```

#### Delete deployment: 
```kubectl delete deployment [name]```

#### Status of different K8s components:
```kubectl get nodes
kubectl get pod
kubectl get pod -o wide
kubectl get services
kubectl get replicaset
kubectl get deployment
kubectl get secret
```

#### Debugging pods:
```kubectl logs [pod name]
kubectl exec -ti [podname] -- bin/bash
kubectl describe pod [pod name]
kubectl get deployment [deployment name] -o yaml > nginx-deplolyment-result.yaml
```
#### Using config file:
```kubectl apply -f [filename]
kubectl delete -f [filename]
```
#### Assign external IP:
```minikube service mongo-express-service```

#### namespaces:
```kubectl create namespace [name]```

#### Other commands:
```kubectl cluster-info```

#### References
Kubernetes tutorial [https://www.youtube.com/watch?v=X48VuDVv0do]