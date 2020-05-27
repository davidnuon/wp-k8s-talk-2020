# Kubernetes, WordPress and You

These are the notes for the talk I gave at WordPress LA, Long Beach Chapter. 

Do not use this as advice for a production facing WordPress setup.

## Setup
We're going to assume to have the following installed and configured:

- doctl - DigitalOcean management tool
  - Be sure to run `doctl auth init` to setup your security token
- kubectl - K8s management tool (Kube Control)
- helm - K8s package manager

Run the following to set up the cluster 
```
doctl kubernetes cluster create wp-k8s-talk --count=2
kubectl -n kube-system create serviceaccount tiller
kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm install external-dns bitnami/external-dns -f externaldns-values.yaml
```

Then, run the following to install Bitnami's WordPress Chart

```
helm install wordpress bitnami/wordpress -f wordpress-values.yaml
```

When you're done with your cluster, destroy it with the following
```
doctl kubernetes cluster delete wp-k8s-talk
```
