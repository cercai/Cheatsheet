# CRICTL

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

List the containers created by kind
```sh
$ docker ps                        
CONTAINER ID   IMAGE                  COMMAND                  CREATED             STATUS             PORTS                       NAMES
fd3cf945593d   kindest/node:v1.30.0   "/usr/local/bin/entr…"   About an hour ago   Up About an hour   127.0.0.1:39311->6443/tcp   k8s-control-plane
0e6e7c1d59ed   kindest/node:v1.30.0   "/usr/local/bin/entr…"   About an hour ago   Up About an hour                               k8s-worker2
ef90b14a642c   kindest/node:v1.30.0   "/usr/local/bin/entr…"   About an hour ago   Up About an hour                               k8s-worker
```

Enter in the control-plane container and look at the pods inside
```console
$ docker exec -it k8s-control-plane bash

root@k8s-control-plane:/# crictl ps
CONTAINER           IMAGE               CREATED             STATE               NAME                      ATTEMPT             POD ID              POD
a9a5552195d13       cbb01a7bd410d       About an hour ago   Running             coredns                   0                   d99910cea850a       coredns-7db6d8ff4d-th4zf
f1aa27a779287       cbb01a7bd410d       About an hour ago   Running             coredns                   0                   9308d1405a129       coredns-7db6d8ff4d-87fkf
a3997d5fa7685       0500518ebaa68       About an hour ago   Running             local-path-provisioner    0                   375f2ac31058a       local-path-provisioner-988d74bc-jsgvk
8f706d83f22be       4950bb10b3f87       About an hour ago   Running             kindnet-cni               0                   77249eb80a7d6       kindnet-9vsqx
41d39c653038c       c4e12eee82f28       About an hour ago   Running             kube-proxy                0                   2ea482f9272e3       kube-proxy-z4zqn
97ef012734d79       3861cfcd7c04c       About an hour ago   Running             etcd                      0                   9a93f66cb895d       etcd-k8s-control-plane
f013603dbbbba       7f6c51674d5ef       About an hour ago   Running             kube-apiserver            0                   38d95fe08360c       kube-apiserver-k8s-control-plane
86ca61836ae61       6abc94235f022       About an hour ago   Running             kube-controller-manager   0                   381d14e69bc97       kube-controller-manager-k8s-control-plane
e3df6bd78b7dc       6c97f001b028e       About an hour ago   Running             kube-scheduler            0                   310eefd10a770       kube-scheduler-k8s-control-plane
```





