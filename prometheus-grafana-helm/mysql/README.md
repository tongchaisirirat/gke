gcloud container --project "devops-sandbox-2023" clusters create-auto "autopilot-cluster-mind-dev" --region "asia-southeast1" --release-channel "regular" --network "projects/devops-sandbox-2023/global/networks/default" --subnetwork "projects/devops-sandbox-2023/regions/asia-southeast1/subnetworks/default" --cluster-ipv4-cidr "/17"

kubectl get pods -n monitoring

kubectl exec -it mysql-deployment-c47ff79fb-rj25t -n monitoring -- sh

mysql -p
show databases;
use database-wordpress;
select * from books;


kubectl port-forward -n monitoring  mysql-deployment-c47ff79fb-rj25t 3306:3306

kubectl apply -f mysql-configmap.yaml
kubectl apply -f mysql-deployment.yaml
kubectl apply -f mysql-service-dev.yaml
