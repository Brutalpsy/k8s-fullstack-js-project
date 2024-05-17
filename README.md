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

## we can create a secret (imperative command, without using config file) just like a pod or deployment

kubectl create secret generic <secret-name> --from-literal key=value

e.g

kubectl create secret generic pgpassword --from-literal PGPASSWORD=12345asdf

## there are multiple secret types like

kubectl create secret generic <secret-name> --from-literal key=value <br />
kubectl create secret tls <secret-name> --from-literal key=value (https stuff) <br />
kubectl create secret docker-registry <secret-name> --from-literal key=value (with some costom authentication for docker images)
