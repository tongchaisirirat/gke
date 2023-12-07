# Create gke cluster
gcloud container --project "devops-sandbox-2023" clusters create-auto "autopilot-cluster-mind-dev" --region "asia-southeast1" --release-channel "regular" --network "projects/devops-sandbox-2023/global/networks/default" --subnetwork "projects/devops-sandbox-2023/regions/asia-southeast1/subnetworks/default" --cluster-ipv4-cidr "/17"

# connect cluster
gcloud container clusters get-credentials autopilot-cluster-mind-dev --region asia-southeast1 --project devops-sandbox-2023

# create namespace
kubectl create namespace dev
kubectl create namespace prod

# create configmap
kubectl apply -f configmap-dev.yaml


# create volumeclaim
kubectl apply -f mysql-volumeclaim.yaml
kubectl apply -f wordpress-volumeclaim.yaml

# create mysql and expose service
kubectl apply -f mysql-deployment.yaml
kubectl apply -f mysql-service-dev.yaml

# for communication acroos namespace
kubectl apply -f mysql-service-prod.yaml


# create wordpress and expose service
kubectl apply -f wordpress-deployment-prod.yaml
kubectl apply -f wordpress-service-prod.yaml


kubectl get pods -n dev
kubectl get pods -n prod

kubectl exec -it pods/mysql-deployment-5749c5f79d-gpgzl -n dev -- bash
kubectl exec -it pods/wordpress-deployment-prod-5dd668cd99-vxmcv -n prod -- bash
# show mount disk
lsblk


