# Argo-cd

To create a k3d cluster with yaml file

kind: Simple
apiVersion: k3d.io/v1alpha2
name: k3d-cluster
image: rancher/k3s:v1.20.4-k3s1
servers: 1
agents: 2
ports:
  - port: 8080:80
    nodeFilters:
      - loadbalancer

command:
k3d cluster create --config k3d.yaml 

To install Argo-cd in cluster:

kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


kubectl port-forward svc/argocd-server -n argocd 8081:443 --address 0.0.0.0

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo