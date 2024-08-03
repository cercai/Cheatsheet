## Create the cluster

Create a cluster with kind or however you want
```sh
$ kind create cluster --name k8s --config config-1.yaml           
Creating cluster "k8s" ...
 ✓ Ensuring node image (kindest/node:v1.30.0)
 ...
```

## Make sure you use the right cluster

Verify you use the right cluster
```sh
$ kubectl config get-contexts
CURRENT   NAME       CLUSTER    AUTHINFO   NAMESPACE
*         kind-k8s   kind-k8s   kind-k8s
```

Use `kubectl config use-context` to change the context,<br>
and `kubectl config use-cluster` to change the cluster.

## List the nodes,pods

```sh
$ kubectl get nodes
NAME                STATUS   ROLES           AGE   VERSION
k8s-control-plane   Ready    control-plane   63s   v1.30.0
k8s-worker          Ready    <none>          42s   v1.30.0
k8s-worker2         Ready    <none>          42s   v1.30.0

$ kubectl get nodes -o wide
NAME                STATUS   ROLES           AGE     VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                         KERNEL-VERSION   CONTAINER-RUNTIME
k8s-control-plane   Ready    control-plane   6m7s    v1.30.0   172.18.0.4    <none>        Debian GNU/Linux 12 (bookworm)   6.8.11-amd64     containerd://1.7.15
k8s-worker          Ready    <none>          5m46s   v1.30.0   172.18.0.2    <none>        Debian GNU/Linux 12 (bookworm)   6.8.11-amd64     containerd://1.7.15
k8s-worker2         Ready    <none>          5m46s   v1.30.0   172.18.0.3    <none>        Debian GNU/Linux 12 (bookworm)   6.8.11-amd64     containerd://1.7.15
```
List the pods

```sh
$ kubectl get pods --all-namespaces
NAMESPACE            NAME                                        READY   STATUS    RESTARTS   AGE
kube-system          coredns-7db6d8ff4d-87fkf                    1/1     Running   0          10m
kube-system          coredns-7db6d8ff4d-th4zf                    1/1     Running   0          10m
kube-system          etcd-k8s-control-plane                      1/1     Running   0          11m
kube-system          kindnet-9vsqx                               1/1     Running   0          10m
kube-system          kindnet-lktrn                               1/1     Running   0          10m
kube-system          kindnet-mpfnv                               1/1     Running   0          10m
kube-system          kube-apiserver-k8s-control-plane            1/1     Running   0          11m
kube-system          kube-controller-manager-k8s-control-plane   1/1     Running   0          11m
kube-system          kube-proxy-rx4f2                            1/1     Running   0          10m
kube-system          kube-proxy-sw7fd                            1/1     Running   0          10m
kube-system          kube-proxy-z4zqn                            1/1     Running   0          10m
kube-system          kube-scheduler-k8s-control-plane            1/1     Running   0          11m
local-path-storage   local-path-provisioner-988d74bc-jsgvk       1/1     Running   0          10m


$ kubectl get pods --all-namespaces -o wide
NAMESPACE            NAME                                        READY   STATUS    RESTARTS   AGE   IP           NODE                NOMINATED NODE   READINESS GATES
kube-system          coredns-7db6d8ff4d-87fkf                    1/1     Running   0          10m   10.244.0.2   k8s-control-plane   <none>           <none>
kube-system          coredns-7db6d8ff4d-th4zf                    1/1     Running   0          10m   10.244.0.4   k8s-control-plane   <none>           <none>
kube-system          etcd-k8s-control-plane                      1/1     Running   0          11m   172.18.0.4   k8s-control-plane   <none>           <none>
kube-system          kindnet-9vsqx                               1/1     Running   0          10m   172.18.0.4   k8s-control-plane   <none>           <none>
kube-system          kindnet-lktrn                               1/1     Running   0          10m   172.18.0.3   k8s-worker2         <none>           <none>
kube-system          kindnet-mpfnv                               1/1     Running   0          10m   172.18.0.2   k8s-worker          <none>           <none>
kube-system          kube-apiserver-k8s-control-plane            1/1     Running   0          11m   172.18.0.4   k8s-control-plane   <none>           <none>
kube-system          kube-controller-manager-k8s-control-plane   1/1     Running   0          11m   172.18.0.4   k8s-control-plane   <none>           <none>
kube-system          kube-proxy-rx4f2                            1/1     Running   0          10m   172.18.0.2   k8s-worker          <none>           <none>
kube-system          kube-proxy-sw7fd                            1/1     Running   0          10m   172.18.0.3   k8s-worker2         <none>           <none>
kube-system          kube-proxy-z4zqn                            1/1     Running   0          10m   172.18.0.4   k8s-control-plane   <none>           <none>
kube-system          kube-scheduler-k8s-control-plane            1/1     Running   0          11m   172.18.0.4   k8s-control-plane   <none>           <none>
local-path-storage   local-path-provisioner-988d74bc-jsgvk       1/1     Running   0          10m   10.244.0.3   k8s-control-plane   <none>           <none>
```

At first, no worker node is tainted, only the master node is tainted.
```sh
$ kubectl describe nodes | grep -i taint
Taints:             node-role.kubernetes.io/control-plane:NoSchedule
Taints:             <none>
Taints:             <none>
```

Create a deployment with first 1 replica then scale it to 2 replicas.
```sh
# CREATE 1 POD
$ kubectl create deploy nginx-depl --image nginx --replicas 1
deployment.apps/nginx-depl created
# k8s-worker got the pod
$ kubectl get pods -n default -o wide
NAME                          READY   STATUS    RESTARTS   AGE   IP           NODE         NOMINATED NODE   READINESS GATES
nginx-depl-85c9d7c5f4-6s554   1/1     Running   0          18s   10.244.2.2   k8s-worker   <none>           <none>
# RESCALE THE DEPLOYMENT
$ kubectl scale deploy nginx-depl --replicas 2
deployment.apps/nginx-depl scaled
# BOTH WORKER GOT ONE POD
$ kubectl get pods -n default -o wide         
NAME                          READY   STATUS    RESTARTS   AGE     IP           NODE          NOMINATED NODE   READINESS GATES
nginx-depl-85c9d7c5f4-285h8   1/1     Running   0          2m41s   10.244.1.2   k8s-worker2   <none>           <none>
nginx-depl-85c9d7c5f4-6s554   1/1     Running   0          3m21s   10.244.2.2   k8s-worker    <none>           <none>
```


## Taint a worker node

Taint a node
```sh
$ kubectl taint nodes k8s-worker key1=value1:NoSchedule
node/k8s-worker tainted

$ kubectl describe nodes | grep -i taint
Taints:             node-role.kubernetes.io/control-plane:NoSchedule
Taints:             app=nginx:NoSchedule
Taints:             <none>
```

