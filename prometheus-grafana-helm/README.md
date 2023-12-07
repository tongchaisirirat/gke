https://blog.devops.dev/deploy-prometheus-and-grafana-into-your-kubernetes-cluster-easily-c838a4eb956d

# Create gke cluster
gcloud container --project "devops-sandbox-2023" clusters create-auto "autopilot-cluster-mind-dev" --region "asia-southeast1" --release-channel "regular" --network "projects/devops-sandbox-2023/global/networks/default" --subnetwork "projects/devops-sandbox-2023/regions/asia-southeast1/subnetworks/default" --cluster-ipv4-cidr "/17"

# connect cluster
gcloud container clusters get-credentials autopilot-cluster-mind-dev --region asia-southeast1 --project devops-sandbox-2023

# 1. Connect to your Kubernetes Cluste
kubectl get svc -all -o -wide
helm version
## คำสั่ง kubectl get svc --all -o wide ใช้เพื่อดึงข้อมูลเกี่ยวกับ Kubernetes Services ทั้งหมดใน cluster และแสดงผลลัพธ์ในรูปแบบที่กว้างมาก (wide). 

ต่อไปนี้คำอธิบายของตัวอาร์กิวเมนต์ที่ใช้ในคำสั่ง:

get svc: เรียกดูข้อมูลของ Kubernetes Services.
--all หรือ -A: ให้คำสั่งแสดงข้อมูลของทุก Namespace ไม่ว่าจะเป็น default หรือ Namespace อื่น ๆ.
-o wide: ระบุรูปแบบการแสดงผลในรูปแบบที่กว้างมาก เพื่อแสดงรายละเอียดเพิ่มเติมเกี่ยวกับ Services เช่น IP address, ports, age และอื่น ๆ.

# 2. Adding Helm Prometheus
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

# 3. Creating username and password for Grafana
#Remember to change the username!
echo -n 'grafana-admin' > ./grafana-admin-user

#Remember to change the password!
echo -n 'Welcome123' > ./grafana-admin-password

# 4. Create a Namespace
kubectl create namespace monitoring
kubectl get ns

# 5. Create Kubernetes Secret
 kubectl create secret generic grafana-admin-creds --from-file=./grafana-admin-user --from-file=grafana-admin-password -n monitoring

# 6. Create a YAML file
template.yaml

# 7. Install Prometheus from helm
helm install -n monitoring prometheus prometheus-community/kube-prometheus-stack -f template.yaml
kubectl get pods -n monitoring

# 8. Port forward your Grafana pod
kubectl port-forward -n monitoring grafana-f6c486646-wzgdp localhost:3000

kubectl port-forward -n monitoring  grafana-f6c486646-wzgdp 3000:3000


