# Hue on Kubernetes

How to run Hue in Kubernetes.


## Quick Start

Assuming you have a Kubernetes cluster configured with Helm installed and images pushed (if not, check the [K8s Cluster](#K8s_Cluster) section below).

* [Helm](helm)
   * [Hue](helm/hue)
* YAML
   * NGINX (TBD)
   * Task Server (TBD)
* Containers
   * [Hue](services/hue)

## K8s Cluster

### Ubuntu

OS: Ubuntu 16.04 or 18.04.
Node count and size: 4 primary instances of m3.xlarge (4CPU 15GB) or 1 m3.2xlarge (8CPU 30GB).

https://microk8s.io/#quick-start

```
sudo snap install microk8s --classicmicro

snap alias microk8s.kubectl kubectl

k8s.enable metrics-server dns
```

```
sudo snap install helm --classic

helm init
```

If in Dev, for having the provisioner run properly:

```
kubectl create clusterrolebinding serviceaccounts-cluster-admin --clusterrole=cluster-admin --group=system:serviceaccounts
```

### GKE

Install Helm onto GKE cluster requires creating a service account with the correct
permissions:

```
kubectl create serviceaccount --namespace kube-system tiller
kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'
helm init --service-account tiller --upgrade
```

On GKE, this chart uses a LoadBalancer to route to Traefik rather than using the GKE
HTTP LoadBalancer. This avoids creating global static ips.

## Images

All the images can currently can be built via the [services](services).

