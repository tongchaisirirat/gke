kubectl apply -f configmap.yaml

kubectl apply -f mysql-deployment.yaml
kubectl apply -f mysql-service-dev.yaml
kubectl apply -f mysql-service-prod.yaml

kubectl apply -f wordpress-deployment-dev.yaml
kubectl apply -f wordpress-deployment-prod.yaml