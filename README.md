# in order to delete previous deployments and services

kubectl get deployments
kubectl delete deployment <name-of-the-deployment>

kubectl get services
kubectl delete service <name-of-the-service>

## to apply configuration to group of files we can apply to whole folder

kubectl apply -f k8s

## we can get the logs of a specific pod

kubectl get pods
kubectl logs <pod-name>
