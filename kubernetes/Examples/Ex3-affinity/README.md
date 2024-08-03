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


Create a deployment with first 1 replica then scale it to 2 replicas.
```sh
# CREATE 1 POD
$ kubectl apply -f pod-nginx.yaml 
pod/nginx created

$ kubectl get pods -o wide
NAME    READY   STATUS    RESTARTS   AGE   IP       NODE     NOMINATED NODE   READINESS GATES
nginx   0/1     Pending   0          25s   <none>   <none>   <none>           <none>

$ kubectl get nodes --show-labels
NAME                STATUS   ROLES           AGE   VERSION   LABELS
k8s-control-plane   Ready    control-plane   8h    v1.30.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-control-plane,kubernetes.io/os=linux,node-role.kubernetes.io/control-plane=,node.kubernetes.io/exclude-from-external-load-balancers=
k8s-worker          Ready    <none>          8h    v1.30.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-worker,kubernetes.io/os=linux
k8s-worker2         Ready    <none>          8h    v1.30.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-worker2,kubernetes.io/os=linux

$ kubectl label nodes k8s-worker disk=ssd
node/k8s-worker labeled

$ kubectl get nodes -l disk --show-labels
NAME          STATUS   ROLES    AGE   VERSION   LABELS
k8s-worker2   Ready    <none>   8h    v1.30.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,disk=ssd,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-worker2,kubernetes.io/os=linux


$ kubectl get pods -o wide
NAME    READY   STATUS    RESTARTS   AGE     IP           NODE         NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          2m42s   10.244.1.5   k8s-worker2   <none>           <none>
```

