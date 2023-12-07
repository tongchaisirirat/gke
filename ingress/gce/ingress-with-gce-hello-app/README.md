https://cloud.google.com/kubernetes-engine/docs/tutorials/http-balancer

# Create gke cluster
gcloud container --project "devops-sandbox-2023" clusters create-auto "autopilot-cluster-mind-dev" --region "asia-southeast1" --release-channel "regular" --network "projects/devops-sandbox-2023/global/networks/default" --subnetwork "projects/devops-sandbox-2023/regions/asia-southeast1/subnetworks/default" --cluster-ipv4-cidr "/17"

# connect cluster
gcloud container clusters get-credentials autopilot-cluster-mind-dev --region asia-southeast1 --project devops-sandbox-2023


kubectl apply -f web-deployment.yaml
kubectl apply -f web-service.yaml
kubectl apply -f basic-ingress.yaml

kubectl get ingress basic-ingress
