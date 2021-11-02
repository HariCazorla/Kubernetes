## To deploy [Expense-tracker](https://github.com/HariCazorla/expense-tracker) on K8s follow these steps,
First we will deploy the postgres db as a stateful set,

```
kubectl apply -f postgres
```

Deploy expense tracker as a deployment,

```
kubectl apply -f expense-tracker
```

To access swagger ui 
```
minikube service expense-tracker-service
```

## Cleanup

```
kubectl delete -f postgres
kubectl delete -f expense-tracker
```