https://medium.com/google-cloud/deploying-service-or-ingress-on-gke-59a49b134e3b

mind repo


kubectl apply --filename hello_gke_extlb_deploy.yaml
kubectl apply --filename hello_gke_extlb_svc.yaml

kubectl get pods
kubectl get pods -o custom-columns=POD:metadata.name,NODE:spec.nodeName,IP:status.podIP
kubectl get pods -o wide

kubectl get node



kubectl exec -it -n default pod/hello-gke-extlb-78d5f76589-427f2 -- sh

kubectl cp default/hello-gke-extlb-78d5f76589-427f2:/usr/src/app /Users/tongchai-sirirat/Desktop/gke/app

docker build -t gke-check-pod-image . 
docker tag gke-image asia-southeast1-docker.pkg.dev/devops-sandbox-2023/mind-artifact/gke-check-pod-image
docker push  asia-southeast1-docker.pkg.dev/devops-sandbox-2023/mind-artifact/gke-check-pod-image 



kubectl apply -f deployment.yaml
kubectl delete -f deployment.yaml


kubectl apply -f values.yaml


helm search repo ingress-nginx
helm show values ingress-nginx/ingress-nginx

helm search repo ingress-nginx
helm show values ingress-nginx/ingress-nginx

helm upgrade -f values.yaml ingress-nginx ingress-nginx/ingress-nginx