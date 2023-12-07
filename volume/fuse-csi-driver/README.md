https://chimbu.medium.com/access-cloud-storage-buckets-as-volumes-in-gke-c2e405adea6c

# create GKE Cluster
gcloud container clusters create autopilot-cluster-mind-dev \
    --addons GcsFuseCsiDriver \
    --cluster-version=1.27.3-gke.100 \
    --region=asia-southeast1-b \
    --workload-pool=devops-sandbox-2023.svc.id.goog

gcloud container clusters get-credentials autopilot-cluster-mind-dev --zone asia-southeast1-b --project devops-sandbox-2023

# Create bucket
gcloud storage buckets create gs://mind-gcs-fuse-demo-bucket --location=asia-southeast1 --project devops-sandbox-2023

# Create a GCP service account in the Cloud Storage bucket project.
gcloud iam service-accounts create mind-cs-fuse-demo --project=devops-sandbox-2023

# Apply permissions to a specific bucket.
gcloud storage buckets add-iam-policy-binding gs://mind-gcs-fuse-demo-bucket \
  --member "serviceAccount:mind-cs-fuse-demo@devops-sandbox-2023.iam.gserviceaccount.com" \
  --role "roles/storage.objectAdmin" \
  --project devops-sandbox-2023

# Bind the the Kubernetes Service Account with the GCP Service Account. The kubernetes resources will be created in the next step.

gcloud iam service-accounts add-iam-policy-binding mind-cs-fuse-demo@devops-sandbox-2023.iam.gserviceaccount.com \
  --role roles/iam.workloadIdentityUser \
  --member "serviceAccount:devops-sandbox-2023.svc.id.goog[default/cs-fuse-demo-app]" \
  --project devops-sandbox-2023

# apply pv and pvc
kubectl apply -f PersistentVolume.yaml,PersistentVolumeClaim.yaml

kubectl get PersistentVolume
kubectl delete  PersistentVolume/cs-fuse-pv 

kubectl get PersistentVolumeClaim
kubectl delete PersistentVolumeClaim/cs-fuse-pvc

# apply deployment 
kubectl apply -f ServiceAccount.yaml

kubectl get serviceaccount
kubectl delete serviceaccount/cs-fuse-demo-app


# apply deployment 
kubectl apply -f deployment.yaml

kubectl get deployment
kubectl delete deployment/cs-fuse-demo-app

# get pods and exec pods

kubectl get pods

kubectl delete pods/cs-fuse-demo-app-ddccf74df-jhgn4


kubectl exec -it cs-fuse-demo-app-ddccf74df-lfqks -c busybox sh
cd /data/
ls -lrt
