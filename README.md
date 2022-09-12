# k8s-volt-training-handson

> Exercises to build confidence in deploying and maintaining Volt Clusters with Kubernetes.
---

This training will be carried out on Google Kubernetes Engine(GKE) which is a service of Google cloud platform. GKE provides the kubernetes cluster which is the base for deploying Volt. 
However, the steps and operations carried out on volt cluster are not limited to GKE and GCP. The kubernetes cluster can be hosted on public cloud or private cloud, the tools used to manage volt cluster on kubernetes are the industry standard KUBECTL, and HELM.

### Pre-requisites

Tools needed on participant's system:
- [x] Gloud CLI : to interact with gcloud and GKE
- [x] kubectl CLI : kubernetes client to call kubernetes server APIs to execute commands from local terminal
- [x] Helm 3 CLI : Helm client to enable helm operations from local terminal

The above tools can be installed following the links provided below. The links are official documents and cover the steps for all possible operating systems.

:link: [Gcloud](https://cloud.google.com/sdk/docs/install)

:link: [Kubectl](https://kubernetes.io/docs/tasks/tools/)

:link: [helm](https://helm.sh/docs/intro/install/)

### Setup
Once installation is done, we are going to check if the installed tools are working fine.
Follow the steps below to add access to GCP project and kubernetes cluster, that will be used in this training. Run the commands on your terminal.

A successful execution will look like this,
```
gcloud auth login
gcloud config set account <<your-email-id>>
gcloud config set project amdocs-volt-training

$ gcloud container clusters get-credentials training-yellow-pheonix --zone us-central1-a --project amdocs-volt-training
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

This ensures gcloud access, kubernetes cluster and kubectl are working as we expect and we can move to the next step.

The latest helm chart, v1.8.2, will be used for this training. It can be downloaded from the buckets of the project. 
Google buckets name : volt-training
Download the tar and extract it to get your training helm chart.

The difference in syntax for local helm charts is,

`helm install CHART-FOLDER/ --set your.properties`


---
### Installing Volt Cluster

We are getting into the real stuff :test_tube:
Spinning up a Volt cluster that has the following helm properties defined by the user.
#### Lab 1 
1. Lets build the required helm install command with the following properties defined by the user,

- Replica count is 3 
- Resources request and limits both are set to 4vCPU and 8GB RAM
- Docker image repo and tag

The relevant documentation can be found :link: [here](https://docs.voltdb.com/KubernetesAdmin/HelmConfigK8sStartup.php#helmconfigtab1)

2. Now to add more complexity, lets define the following properties too in the same command,

- k-factor
- Sites per host
- Command log
- Snapshot

> Remember, if the command is too big to manage, we can always create a properties YAML file having all the user defined helm properties in correct heirarchy. 

The relevant documentation can be found :link: [here](https://docs.voltdb.com/KubernetesAdmin/HelmConfigDB.php)

3. Finally, execute the `helm install` command you built and spin up your very own volt cluster.
Attach the yaml file and helm install command in your respective solution documents.

### Verify Installation
#### Lab 2
Lets verify the configurations were picked up and implemented correctly. 
Please check status of following entities and attach output to your solution documents.
Pods, services, statefulsets and deployments, replicaset and volt-logs

Happy scenario things will looks something like this,
1. Pods
```
$ kubectl get pods -n train-2
NAME                                     READY   STATUS    RESTARTS   AGE
mydb2-voltdb-cluster-0                   1/1     Running   0          2m8s
mydb2-voltdb-cluster-1                   1/1     Running   0          2m8s
mydb2-voltdb-cluster-2                   1/1     Running   0          2m8s
mydb2-voltdb-operator-79d4cb5b78-t4dgw   1/1     Running   0          2m15s

$ kubectl get pods -n train-2 -o wide
NAME                                     READY   STATUS    RESTARTS   AGE     IP            NODE                                                  NOMINATED NODE   READINESS GATES
mydb2-voltdb-cluster-0                   1/1     Running   0          2m13s   10.112.1.13   gke-training-yellow-pheo-default-pool-a05d229c-n3wt   <none>           <none>
mydb2-voltdb-cluster-1                   1/1     Running   0          2m13s   10.112.0.18   gke-training-yellow-pheo-default-pool-a05d229c-1jff   <none>           <none>
mydb2-voltdb-cluster-2                   1/1     Running   0          2m13s   10.112.2.14   gke-training-yellow-pheo-default-pool-a05d229c-xxjj   <none>           <none>
mydb2-voltdb-operator-79d4cb5b78-t4dgw   1/1     Running   0          2m20s   10.112.0.17   gke-training-yellow-pheo-default-pool-a05d229c-1jff   <none>           <none>

```
2. Services
```
$ kubectl get services -n train-2
NAME                            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)                                                   AGE
mydb2-voltdb-cluster-client     ClusterIP   10.116.12.98   <none>        21211/TCP,21212/TCP                                       2m54s
mydb2-voltdb-cluster-http       ClusterIP   10.116.1.142   <none>        8080/TCP                                                  2m54s
mydb2-voltdb-cluster-internal   ClusterIP   None           <none>        8080/TCP,3021/TCP,7181/TCP,9090/TCP,11235/TCP,11780/TCP   2m54s
voltdb-operator-metrics         ClusterIP   10.116.15.23   <none>        8383/TCP,8686/TCP                                         2m55s

```
3. Statefulsets, deployment, and replicaset
```
$ kubectl get statefulsets -n train-2
NAME                   READY   AGE
mydb2-voltdb-cluster   3/3     3m16s

$ kubectl get deployments -n train-2
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
mydb2-voltdb-operator   1/1     1            1           3m36s

$ kubectl get replicaset -n train-2
NAME                               DESIRED   CURRENT   READY   AGE
mydb2-voltdb-operator-79d4cb5b78   1         1         1       4m18s

```

4. Volt Logs
```
$ kubectl logs -f mydb2-voltdb-cluster-1 -n train-2
2022-09-08 09:36:48,978 INFO     voltboot  ==== VoltDB startup controller for Kubernetes ====
2022-09-08 09:36:48,978 INFO     voltboot  GMT is: 2022-09-08 09:36:48 LOCALTIME is: 2022-09-08 09:36:48 TIMEZONE OFFSET is: 0
2022-09-08 09:36:48,978 INFO     voltboot  Running under Python 3.9.5
2022-09-08 09:36:48,978 INFO     voltboot  VoltDB version is 11.4.1
2022-09-08 09:36:48,978 INFO     voltboot  Host: mydb2-voltdb-cluster-1.mydb2-voltdb-cluster-internal.train-2.svc.cluster.local
2022-09-08 09:36:48,978 INFO     voltboot  Pod: mydb2-voltdb-cluster-1
2022-09-08 09:36:48,978 INFO     voltboot  Using logging config file: /opt/voltdb/tools/kubernetes/server-log4j.xml
2022-09-08 09:36:48,978 INFO     voltboot  Working directory is: /pvc/voltdb
2022-09-08 09:36:48,979 INFO     voltboot  *** VoltDB state is now 'uninitialized' ***
2022-09-08 09:36:48,985 INFO     voltboot  IPv6 is disabled in kernel
2022-09-08 09:36:48,985 INFO     voltboot  Ready for commands; current state is 'uninitialized'
2022-09-08 09:36:52,541 INFO     voltboot  Start command received
2022-09-08 09:36:52,541 INFO     voltboot  Initializing a new VoltDB database in '/pvc/voltdb'
......
```
### Verify configuration file

Decode the init secret for your deployment to verify the relevant file created is as exepected. 
```
$ kubectl get secrets -n train-2
NAME                                TYPE                                  DATA   AGE
default-token-cxxfd                 kubernetes.io/service-account-token   3      79m
dockerio-registry                   kubernetes.io/dockerconfigjson        1      41m
mydb2-voltdb-cluster-auth           Opaque                                0      23m
mydb2-voltdb-cluster-init           Opaque                                2      23m
mydb2-voltdb-cluster-token-gbxwd    kubernetes.io/service-account-token   3      23m
mydb2-voltdb-operator-token-j8gm4   kubernetes.io/service-account-token   3      23m
sh.helm.release.v1.mydb2.v1         helm.sh/release.v1                    1      23m

```

### Creating Schema

Use provided SQL file to create schema in the cluster. use SQLCMD to access the Database interactively. You can use any one of the various methods described in the training to access SQLCMD.

1. Port forwarding
```
$ kubectl get pods -n train-2
NAME                                     READY   STATUS    RESTARTS   AGE
mydb2-voltdb-cluster-0                   1/1     Running   0          8m57s
mydb2-voltdb-cluster-1                   1/1     Running   0          8m57s
mydb2-voltdb-cluster-2                   1/1     Running   0          8m57s
mydb2-voltdb-operator-79d4cb5b78-t4dgw   1/1     Running   0          9m4s
bash-5.1$ kubectl get svc -n train-2
NAME                            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)                                                   AGE
mydb2-voltdb-cluster-client     ClusterIP   10.116.12.98   <none>        21211/TCP,21212/TCP                                       9m7s
mydb2-voltdb-cluster-http       ClusterIP   10.116.1.142   <none>        8080/TCP                                                  9m7s
mydb2-voltdb-cluster-internal   ClusterIP   None           <none>        8080/TCP,3021/TCP,7181/TCP,9090/TCP,11235/TCP,11780/TCP   9m7s
voltdb-operator-metrics         ClusterIP   10.116.15.23   <none>        8383/TCP,8686/TCP                                         9m8s
bash-5.1$ kubectl port-forward mydb2-voltdb-cluster-0 21212:21212 -n train-2
Forwarding from 127.0.0.1:21212 -> 21212
Forwarding from [::1]:21212 -> 21212
Handling connection for 21212

new Tab: 
Monterey:bin thanos$ ./sqlcmd
SQL Command :: localhost:21212
1> load classes voter-procs.jar;
Command succeeded.
2> file  ddl.sql;
Batch command succeeded.
Batch command succeeded.
Command succeeded.
Batch command succeeded.
3>
```
2. Run some ad-hoc queries to check the schema you just loaded.
```
 $ kubectl exec -it mydb2-voltdb-cluster-1 -n train-2 -- sqlcmd
SQL Command :: localhost:21212
1> show tables;

--- Tables ---------------------------------------------------
AREA_CODE_STATE
CONTESTANTS
VOTES


--- DR Tables ------------------------------------------------


--- Streams --------------------------------------------------


--- Views ----------------------------------------------------
V_VOTES_BY_CONTESTANT_NUMBER_STATE
V_VOTES_BY_PHONE_NUMBER

```
Attach your respective outputs in your solution documents. 
### Restarting cluster

Lets restart the entire cluster, by specifying replicas=0 
then back to original number.

The relevant documentation can be found :link: [here](https://docs.voltdb.com/KubernetesAdmin/OpsStopCluster.php) 
### Pause and resume

Use Helm voltadmin to pause the cluster, then use it again to resume the cluster.

### Making changes to the deployment

Changes in volt are made using helm upgrade and helm properties. for our practice, we are going to change two properties of your environment.
1. Adding export configurations - no restart required
2. Enable Command logging - Restart required

The relevant documentation can be found :link: [here](https://docs.voltdb.com/KubernetesAdmin/OpsUpdateClusterConfig.php#UpdateDbRestart)


### XDCR

Let us begin a XDCR setup with 2 clusters in different regions, using PER-POD node port configuraion.
Replicate the environment outlined in the presentation chapter 05 XDCR scenario 1.


Verify XDCR is setup correctly by checking the various statistics and log messages.
```
exec @Statistics DRROLE 0;
exec @Statistics DRCONSUMER 0;
exec @Statistics DRPRODUCER 0;
exec @Statistics XDCR_READINESS 0;
