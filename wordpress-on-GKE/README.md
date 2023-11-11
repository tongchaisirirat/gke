### 0.Setting up your environment

https://cloud.google.com/kubernetes-engine/docs/tutorials/persistent-disk


# enable the GKE and Cloud SQL Admin APIs
gcloud services enable container.googleapis.com sqladmin.googleapis.com

# 
gcloud config set compute/region asia-southeast1

# 
export PROJECT_ID=devops-sandbox-2023


# Set the WORKING_DIR environment variable
WORKING_DIR=$(pwd)

### ----------------------------------

### 1.Creating a GKE cluster
https://cloud.google.com/kubernetes-engine/docs/tutorials/persistent-disk#creating_a_gke_cluster

CLUSTER_NAME=persistent-disk-tutorial
gcloud container clusters create-auto $CLUSTER_NAME

gcloud container clusters get-credentials $CLUSTER_NAME --region asia-southeast1

### 2.Creating a PV and a PVC backed by Persistent Disk
https://cloud.google.com/kubernetes-engine/docs/tutorials/persistent-disk#creating-a-pv-and-a-pvc-back-by=persistent-disks

kubectl apply -f $WORKING_DIR/wordpress-volumeclaim.yaml
kubectl get persistentvolumeclaim

### 3.Creating a Cloud SQL for MySQL instance
https://cloud.google.com/kubernetes-engine/docs/tutorials/persistent-disk#creating_a_cloud_sql_for_mysql_instance

INSTANCE_NAME=mysql-wordpress-instance
gcloud sql instances create $INSTANCE_NAME

export INSTANCE_CONNECTION_NAME=$(gcloud sql instances describe $INSTANCE_NAME \
    --format='value(connectionName)')

gcloud sql databases create wordpress --instance $INSTANCE_NAME

CLOUD_SQL_PASSWORD=$(openssl rand -base64 18)
gcloud sql users create wordpress --host=% --instance $INSTANCE_NAME \
    --password $CLOUD_SQL_PASSWORD

### 4.Deploying WordPress
https://cloud.google.com/kubernetes-engine/docs/tutorials/persistent-disk#deploying_wordpress

SA_NAME=cloudsql-proxy
gcloud iam service-accounts create $SA_NAME --display-name $SA_NAME

SA_EMAIL=$(gcloud iam service-accounts list \
    --filter=displayName:$SA_NAME \
    --format='value(email)')

gcloud projects add-iam-policy-binding $PROJECT_ID \
    --role roles/cloudsql.client \
    --member serviceAccount:$SA_EMAIL

gcloud projects add-iam-policy-binding $PROJECT_ID \
    --role roles/cloudsql.client \
    --member serviceAccount:$SA_EMAIL

kubectl create secret generic cloudsql-db-credentials \
    --from-literal username=wordpress \
    --from-literal password=$CLOUD_SQL_PASSWORD

kubectl create secret generic cloudsql-instance-credentials \
    --from-file $WORKING_DIR/key.json

### 5.Deploy WordPress
https://cloud.google.com/kubernetes-engine/docs/tutorials/persistent-disk#deploy_wordpress


 cat $WORKING_DIR/wordpress_cloudsql.yaml.template | envsubst > \
    $WORKING_DIR/wordpress_cloudsql.yaml

kubectl create -f $WORKING_DIR/wordpress_cloudsql.yaml

kubectl get pod -l app=wordpress --watch

### 6.Expose the WordPress service
https://cloud.google.com/kubernetes-engine/docs/tutorials/persistent-disk#expose_the_wordpress_service

kubectl create -f $WORKING_DIR/wordpress-service.yaml

kubectl get svc -l app=wordpress --watch