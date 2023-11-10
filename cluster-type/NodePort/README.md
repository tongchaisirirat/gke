https://cloud.google.com/kubernetes-engine/docs/how-to/exposing-apps#creating_a_service_of_type_nodeport

kubectl apply -f my-deployment-50000.yaml
kubectl get pods

kubectl apply -f my-np-service.yaml

kubectl get service my-np-service --output yaml

gcloud compute firewall-rules create test-node-port \
    --allow tcp:NODE_PORT

kubectl get nodes --output wide

http://NODE_IP_ADDRESS:NODE_PORT