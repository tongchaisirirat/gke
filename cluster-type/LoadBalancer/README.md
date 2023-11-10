https://cloud.google.com/kubernetes-engine/docs/how-to/exposing-apps#creating_a_service_of_type_loadbalancer

kubectl apply -f my-deployment-50001.yaml
kubectl get pods

kubectl apply -f my-lb-service.yaml

kubectl get service my-lb-service --output yaml

http://external-ip:60000