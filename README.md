gcloud container --project "devops-sandbox-2023" clusters create-auto "autopilot-cluster-mind-dev" --region "asia-southeast1" --release-channel "regular" --network "projects/devops-sandbox-2023/global/networks/default" --subnetwork "projects/devops-sandbox-2023/regions/asia-southeast1/subnetworks/default" --cluster-ipv4-cidr "/17"

# gke
export PROJECT_ID=project-id

# Set the WORKING_DIR environment variable
WORKING_DIR=$(pwd)

kubectl get pods --watch

## Note --------------------------
# รายการ การ access เข้า gke
kubectl config get-contexts

kubectl config unset current-context
# ลบ การเข้า gke
kubectl config delete-context <context-name>

## --------------------------------
kubectl apply -f filename.yaml

kubectl get deployment
kubectl get pods
kubectl get namespace

kubectl get service
kubectl get services --all-namespaces

# แสดงรายละเอียดของ node
kubectl get nodes --output wide

## --------------------------------
# แสดงรายละเอียดของ service ที่ deploy ไป
kubectl get service my-lb-service --output yaml

## --------------------------------
# ลบ deployment
kubectl delete deployments 
# ลบ service
kubectl delete services my-cip-service