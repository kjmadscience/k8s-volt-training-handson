# k8s-volt-training-handson

# Volt Active Data With Kubernetes
> Exercises to build confidence in deploying and maintaining Volt Clusters with Kubernetes.
---

This training will be carried out on Google Kubernetes Engine(GKE) which is a service of Google cloud platform. GKE provides the kubernetes cluster which is the base for deploying Volt. However, the steps and operations carried out on volt cluster remain standard over all kubernetes clusters. The kubernetes cluster can be hosted on public cloud or private cloud, the tools used to manage volt cluster on kubernetes are the industry standard KUBECTL, and HELM.

### Pre-requisites and Setup

Tools needed on participant's system:
- [x] Gloud CLI : to interact with gcloud and GKE 
- [x] kubectl CLI : kubernetes client to call kubernetes server APIs to execute commands from local terminal
- [x] Helm CLI : Helm client to enable helm operations from local terminal

The above tools can be installed following the links provided below. The links are official documents and cover the steps for all possible operating systems.

[Gcloud](https://cloud.google.com/sdk/docs/install)
[Kubectl](https://kubernetes.io/docs/tasks/tools/)
[helm](https://helm.sh/docs/intro/install/)


Once installation is done, we are going to check if the installed tools are working fine.
You will be provided with a `gcloud` command containing access details to your kubernetes cluster which will be used during this training. Run the command on your terminal.

A successful execution will look like this,
```
bash-5.1$ gcloud container clusters get-credentials training-yellow-pheonix --zone europe-west1-c --project fourth-epigram-293718
Fetching cluster endpoint and auth data.
kubeconfig entry generated for training-yellow-pheonix.
```
Lets check if the cluster assigned to you is well and ready to begin with, we will run some commands to get some information of the k8s cluster.

Lets check the nodes, Pods, namespaces. Below are some example outputs of a happy scenario.
Each participant will have their own namespace, allowing isolation of environments while we all work on the training kubernetes cluster.
```
bash-5.1$ kubectl get nodes
NAME                                                  STATUS   ROLES    AGE     VERSION
gke-training-yellow-pheo-default-pool-f8e7add9-3pbr   Ready    <none>   6m11s   v1.22.11-gke.400
gke-training-yellow-pheo-default-pool-f8e7add9-5k25   Ready    <none>   6m11s   v1.22.11-gke.400
gke-training-yellow-pheo-default-pool-f8e7add9-z1r7   Ready    <none>   6m11s   v1.22.11-gke.400


bash-5.1$ kubectl get nodes -o wide
NAME                                                  STATUS   ROLES    AGE     VERSION            INTERNAL-IP     EXTERNAL-IP      OS-IMAGE             KERNEL-VERSION   CONTAINER-RUNTIME
gke-training-yellow-pheo-default-pool-f8e7add9-3pbr   Ready    <none>   6m15s   v1.22.11-gke.400   10.132.15.202   34.76.45.30      Ubuntu 20.04.4 LTS   5.4.0-1076-gke   containerd://1.5.2
gke-training-yellow-pheo-default-pool-f8e7add9-5k25   Ready    <none>   6m15s   v1.22.11-gke.400   10.132.15.209   34.140.31.61     Ubuntu 20.04.4 LTS   5.4.0-1076-gke   containerd://1.5.2
gke-training-yellow-pheo-default-pool-f8e7add9-z1r7   Ready    <none>   6m15s   v1.22.11-gke.400   10.132.15.206   104.155.58.166   Ubuntu 20.04.4 LTS   5.4.0-1076-gke   containerd://1.5.2
```
```

bash-5.1$ kubectl get namespace
NAME              STATUS   AGE
default           Active   12m
kube-node-lease   Active   12m
kube-public       Active   12m
kube-system       Active   12m
training-1        Active   48s
volt              Active   76s
```

```
bash-5.1$ kubectl get pods
No resources found in default namespace.

bash-5.1$ kubectl get pods -n training-1
No resources found in training-1 namespace.
```

This ensures gcloud access, kubernetes cluster and kubectl are working as we expect and we can move to the next step of verifying Helm Repo access.

