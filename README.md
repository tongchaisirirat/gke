# gke

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