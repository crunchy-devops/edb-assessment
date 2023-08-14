# Argo CD 
 
## Install argo CD server
```shell script
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```
## install argocd cli
```shell script
 VERSION=$(curl --silent "https://api.github.com/repos/argoproj/argo-cd/releases/latest" | grep '"tag_name"' | sed -E 's/.*"([^"]+)".*/\1/')
sudo curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/$VERSION/argocd-linux-amd64
sudo chmod +x /usr/local/bin/argocd
argocd version 
```

## Get Argo CD Password ,
```shell
user is admin and  password is admin
kubectl -n argocd patch secret argocd-secret -p '{"stringData": {
"admin.password": "$2a$10$fulVqTM1sbnreOjh9NemLepWXW4ZPCjP/LjfAJ4aHL2JWVXL8OLVe",
"admin.passwordMtime": "'$(date +%FT%T%Z)'"
}}'
```
## Use KinD
```shell
# access from localhost
kubectl port-forward -n argocd service/argocd-server 3000:443 --address='0.0.0.0'
```