# Argocd Overview
## Install Argo CD
1.Install Argo CD
<pre>kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml</pre>
2.Access The Argo CD API Server

`kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'`

`kubectl port-forward svc/argocd-server -n argocd 8080:443`

3.Login Using The CLI

`argocd admin initial-password -n argocd`

## Create new app in ArgoCD
