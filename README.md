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

## If you are using Docker Desktop's built-in Kubernetes, setting up the admin dashboard is going to take a little more work.

## 1. Grab the most current script from the install instructions:

    https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/#deploying-the-dashboard-ui

eg:
As of today, the kubectl apply command looks like this:
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.5.0/aio/deploy/recommended.yaml

## 2. Create a dash-admin-user.yaml file and paste the following:

      apiVersion: v1
      kind: ServiceAccount
      metadata:
        name: admin-user
        namespace: kubernetes-dashboard

## 3. Apply the dash-admin-user configuration:

kubectl apply -f dash-admin-user.yaml

## 4. Create dash-clusterrole-yaml file and paste the following:

      apiVersion: rbac.authorization.k8s.io/v1
      kind: ClusterRoleBinding
      metadata:
        name: admin-user
      roleRef:
        apiGroup: rbac.authorization.k8s.io
        kind: ClusterRole
        name: cluster-admin
      subjects:
        - kind: ServiceAccount
          name: admin-user
          namespace: kubernetes-dashboard

## 5. Apply the ClusterRole configuration:

    kubectl apply -f dash-clusterrole.yaml

## 6. In the terminal, run kubectl proxy

You must keep this terminal window open and the proxy running!

## 7. Visit the following URL in your browser to access your Dashboard:

    http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

## 8. Obtain the token for this user by running the following in your terminal:

First, run kubectl version in a new terminal window.

If your Kubernetes server version is v1.24 or higher you must run the following command:

    kubectl -n kubernetes-dashboard create token admin-user

## If your Kubernetes server version is older than v1.24 you must run the following command:

    kubectl -n kubernetes-dashboard get secret $(kubectl -n kubernetes-dashboard get sa/admin-user -o jsonpath="{.secrets[0].name}") -o go-template="{{.data.token | base64decode}}"

## 9. Copy the token from the above output and use it to log in at the dashboard.

Be careful not to copy any extra spaces or output such as the trailing % you may see in your terminal.

## 10. After a successful login, you should now be redirected to the Kubernetes Dashboard.

The above steps can be found in the official documentation:

https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md

## to use kubectl inside the google cloud shell

        gcloud config set <project-id-of-gcloud>
        gcloud config set compute/zone <your-cluster-zone (eg. europe-west1-b)>
        gcloud container clusters get-credentials <name-of-your-cluster>

## In order to install helm inside google cloud shell that is needed to install ingress-nginx ingress-nginx, run these commands in gcloud shell

        curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
        chmod 700 get_helm.sh
        ./get_helm.sh

## installing Ingress Nginx using Helm

        helm upgrade --install ingress-nginx ingress-nginx \
        --repo https://kubernetes.github.io/ingress-nginx \
        --namespace ingress-nginx --create-namespace

## Check that your Ingress Controller was deployed:

        kubectl get service ingress-nginx-controller --namespace=ingress-nginx
