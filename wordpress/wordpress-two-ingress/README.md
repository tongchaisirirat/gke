https://medium.com/globant/kubernetes-deployment-deploying-mysql-databases-on-the-gke-8fa675d3d8a


gcloud container --project "devops-sandbox-2023" clusters create-auto "autopilot-cluster-mind-dev" --region "asia-southeast1" --release-channel "regular" --network "projects/devops-sandbox-2023/global/networks/default" --subnetwork "projects/devops-sandbox-2023/regions/asia-southeast1/subnetworks/default" --cluster-ipv4-cidr "/17"

kubectl create namespace dev
kubectl create namespace prod
kubectl get namespace 
echo ‘winter’ | base64

kubectl create -f mysql-secret.yaml
kubectl describe secret mysql-secret -n dev
kubectl get secret mysql-secret -n dev -o yaml
kubectl delete -f mysql-secret.yaml

kubectl create -f mysql-persistent-volume-claim.yaml
kubectl describe pvc mysql-data-store -n dev


kubectl create -f mysql-config-map.yaml
kubectl describe configmap mysql-preload-data-config -n dev

kubectl create -f mysql-deployment.yaml
kubectl describe deployment mysql-deploy -n dev


kubectl create -f mysql-service.yaml
kubectl describe service mysql-service -n dev

kubectl config set-context --current --namespace=dev

kubectl get pods
kubectl exec --stdin --tty pod/mysql-deploy-545c9dcd7f-c6knl -- /bin/bash
mysql -p
show databases;
use book-management-db;
select * from books;


kubectl exec --stdin --tty pod/wordpress-74d79dfdfd-xd6ds -- /bin/bash


kubectl create -f mysql-secret.yaml
kubectl apply -f mysql-volumeclaim.yaml
kubectl apply -f mysql.yaml
kubectl apply -f mysql-service.yaml

kubectl apply -f wordpress-volumeclaim.yaml
kubectl apply -f wordpress-deployment.yaml
kubectl apply -f wordpress-service.yaml


## -----------------------------------------------

# To delete deployments,services,pods,replicasets
kubectl delete all --all -n dev

# To delete the configmap mysql-preload-data-config
kubectl delete configmap/mysql-preload-data-config -n dev

# To delete the secret mysql-secret
kubectl delete secret/mysql-secret -n dev

# To delete the persistent volum claim mysql-data-store
kubectl delete persistentvolumeclaim/mysql-data-store -n dev