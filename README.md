# homek8s-argo

Christian's attempt at ArgoCD-driven GitOps and the App of apps pattern for use with his home Kubernetes cluster and other demos

The approach documented her is meant to be centered around ArgoCD, so steps will be taken to try and use as few other tools as possible.


## Cluster Assumptions

The clusters to be installed are expected to be bare clusters bootstrapped using Kubeadm backed by the Weave CNI. These are tested against these scripts:

- Control plane
  - Debian/Ubuntu: https://raw.githubusercontent.com/RX-M/classfiles/master/k8s.sh
  - Centos/Rocky: https://raw.githubusercontent.com/RX-M/classfiles/master/k8s-centos-control.sh
- Worker node
  - Debian/Ubuntu: https://raw.githubusercontent.com/RX-M/classfiles/master/k8s-node-only.sh
  - Centos/Rocky: https://raw.githubusercontent.com/RX-M/classfiles/master/k8s-centos-worker.sh

ArgoCD should already be installed. This repo assumes ArgoCD core was installed per their documentation:

```
export ARGOCD_VERSION=v2.10.0

kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/$ARGOCD_VERSION/manifests/core-install.yaml
```


## The App of Apps
 
The App of Apps Pattern is a specific method of using an instance of an ArgoCD `application` CRD to deploy multiple applications. Per their docs, this is primarily an administrative tactic used in the Cluster boostrap. Per that statement, the of the app of apps being presented here is adminstratively focused and will install:

- Longhorn for CSI to drive persistent volumes: https://longhorn.io/docs/1.6.1/deploy/install/install-with-argocd/
- MetalLB for LoadBalancer support: https://metallb.universe.tf/installation/#installation-with-helm
- Istio for Service Mesh: https://istio.io/latest/docs/setup/install/helm/
