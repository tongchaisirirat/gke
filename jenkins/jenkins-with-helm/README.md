https://artifacthub.io/packages/helm/jenkinsci/jenkins

gcloud container --project "devops-sandbox-2023" clusters create-auto "autopilot-cluster-mind-dev" --region "asia-southeast1" --release-channel "regular" --network "projects/devops-sandbox-2023/global/networks/default" --subnetwork "projects/devops-sandbox-2023/regions/asia-southeast1/subnetworks/default" --cluster-ipv4-cidr "/17"

# 1.Get Repository Info
helm repo add jenkins https://charts.jenkins.io
helm repo update

# 2.Install Chart
# Helm 3
$ helm install [RELEASE_NAME] jenkins/jenkins [flags]

