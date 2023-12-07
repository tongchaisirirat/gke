https://dev.to/peepeepopapapeepeepo/lfs258-8-15-kubernetes-volumes-and-data-17de

# Create gke cluster
gcloud container --project "devops-sandbox-2023" clusters create-auto "autopilot-cluster-mind-dev" --region "asia-southeast1" --release-channel "regular" --network "projects/devops-sandbox-2023/global/networks/default" --subnetwork "projects/devops-sandbox-2023/regions/asia-southeast1/subnetworks/default" --cluster-ipv4-cidr "/17"

# connect cluster
gcloud container clusters get-credentials autopilot-cluster-mind-dev --region asia-southeast1 --project devops-sandbox-2023


kubectl apply -f pv-volume.yaml 
kubectl get pv task-pv-volume


kubectl apply -f pv-claim.yaml
kubectl get pv task-pv-volume
kubectl get pvc task-pv-claim


kubectl apply -f pv-pod.yaml
kubectl get pod task-pv-pod -o wide

# login เข้าไปยังเครื่องที่ pods run อยู่ ในที่นี้คือ kube-0003.novalocal 
$ sudo sh -c "echo 'Hello from Kubernetes storage' > /mnt/data/index.html"
$ cat /mnt/data/index.html
Hello from Kubernetes storage

# กลับไปยัง Master Node
kubectl exec -it task-pv-pod -- /bin/bash
root@task-pv-pod:/# apt update
root@task-pv-pod:/# apt install curl
root@task-pv-pod:/# curl http://localhost/
Hello from Kubernetes storage
root@task-pv-pod:/# exit


# Clean up
kubectl delete pod task-pv-pod
kubectl delete pvc task-pv-claim
kubectl delete pv task-pv-volume