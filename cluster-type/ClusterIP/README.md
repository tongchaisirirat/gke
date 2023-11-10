https://cloud.google.com/kubernetes-engine/docs/how-to/exposing-apps#creating_a_service_of_type_clusterip

##
kubectl apply -f my-deployment.yaml
kubectl get pods

##
kubectl apply -f my-cip-service.yaml

kubectl get service my-cip-service --output yaml
kubectl get pods
kubectl exec -it my-deployment-50000-8876fb655-jc8mn -- sh

##
In your shell, install curl:
apk add --no-cache curl

curl CLUSTER_IP:80
exit

## Note --------------------------
kubectl config get-contexts

kubectl config unset current-context

kubectl config delete-context <context-name>


kubectl get namespace

kubectl get deployment

kubectl get service
kubectl get services --all-namespaces
