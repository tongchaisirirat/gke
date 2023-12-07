https://dev.to/peepeepopapapeepeepo/lfs258-8-15-kubernetes-volumes-and-data-17de

# Create gke cluster
gcloud container --project "devops-sandbox-2023" clusters create-auto "autopilot-cluster-mind-dev" --region "asia-southeast1" --release-channel "regular" --network "projects/devops-sandbox-2023/global/networks/default" --subnetwork "projects/devops-sandbox-2023/regions/asia-southeast1/subnetworks/default" --cluster-ipv4-cidr "/17"

# connect cluster
gcloud container clusters get-credentials autopilot-cluster-mind-dev --region asia-southeast1 --project devops-sandbox-2023

# ตัวอย่างการ mount volume ง่ายๆ คือ emptyDir
kubectl apply -f scratch-volume.yaml
kubectl apply -f busybox-emptyDir.yml
kubectl get pods
kubectl exec -it busybox -- sh
/ # df -h /scratch
/ # exit
kubectl delete -f busybox-emptyDir.yml 
