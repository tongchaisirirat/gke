https://dev.to/peepeepopapapeepeepo/lfs258-8-15-kubernetes-volumes-and-data-17de

# Create gke cluster
gcloud container --project "devops-sandbox-2023" clusters create-auto "autopilot-cluster-mind-dev" --region "asia-southeast1" --release-channel "regular" --network "projects/devops-sandbox-2023/global/networks/default" --subnetwork "projects/devops-sandbox-2023/regions/asia-southeast1/subnetworks/default" --cluster-ipv4-cidr "/17"

# connect cluster
gcloud container clusters get-credentials autopilot-cluster-mind-dev --region asia-southeast1 --project devops-sandbox-2023


#  container ใน pod เดียวกัน จะ share volume กัน
kubectl apply -f busybox-sharedVolume.yml
kubectl get pods

kubectl exec -it busybox -c box -- touch /box/foobar
kubectl exec -it busybox -c busy -- ls /busy/foobar
kubectl delete -f busybox-sharedVolume.yml