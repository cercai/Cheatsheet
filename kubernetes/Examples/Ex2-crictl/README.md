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
```console
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

root@k8s-control-plane:/# crictl pods
POD ID              CREATED             STATE               NAME                                        NAMESPACE            ATTEMPT             RUNTIME
d99910cea850a       About an hour ago   Ready               coredns-7db6d8ff4d-th4zf                    kube-system          0                   (default)
9308d1405a129       About an hour ago   Ready               coredns-7db6d8ff4d-87fkf                    kube-system          0                   (default)
375f2ac31058a       About an hour ago   Ready               local-path-provisioner-988d74bc-jsgvk       local-path-storage   0                   (default)
2ea482f9272e3       About an hour ago   Ready               kube-proxy-z4zqn                            kube-system          0                   (default)
77249eb80a7d6       About an hour ago   Ready               kindnet-9vsqx                               kube-system          0                   (default)
381d14e69bc97       About an hour ago   Ready               kube-controller-manager-k8s-control-plane   kube-system          0                   (default)
9a93f66cb895d       About an hour ago   Ready               etcd-k8s-control-plane                      kube-system          0                   (default)
38d95fe08360c       About an hour ago   Ready               kube-apiserver-k8s-control-plane            kube-system          0                   (default)
310eefd10a770       About an hour ago   Ready               kube-scheduler-k8s-control-plane            kube-system          0                   (default)
```

List images inside the container `k8s-control-plane`
```console
root@k8s-control-plane:/# crictl images
IMAGE                                           TAG                  IMAGE ID            SIZE
docker.io/kindest/kindnetd                      v20240202-8f1494ea   4950bb10b3f87       27.8MB
docker.io/kindest/local-path-helper             v20230510-486859a6   be300acfc8622       3.05MB
docker.io/kindest/local-path-provisioner        v20240202-8f1494ea   0500518ebaa68       19.4MB
registry.k8s.io/coredns/coredns                 v1.11.1              cbb01a7bd410d       18.2MB
registry.k8s.io/etcd                            3.5.12-0             3861cfcd7c04c       57.2MB
registry.k8s.io/kube-apiserver-amd64            v1.30.0              7f6c51674d5ef       89.4MB
registry.k8s.io/kube-apiserver                  v1.30.0              7f6c51674d5ef       89.4MB
registry.k8s.io/kube-controller-manager-amd64   v1.30.0              6abc94235f022       83.5MB
registry.k8s.io/kube-controller-manager         v1.30.0              6abc94235f022       83.5MB
registry.k8s.io/kube-proxy-amd64                v1.30.0              c4e12eee82f28       85.9MB
registry.k8s.io/kube-proxy                      v1.30.0              c4e12eee82f28       85.9MB
registry.k8s.io/kube-scheduler-amd64            v1.30.0              6c97f001b028e       63MB
registry.k8s.io/kube-scheduler                  v1.30.0              6c97f001b028e       63MB
registry.k8s.io/pause                           3.7                  221177c6082a8       311kB
```

configuration file of containerd
```console
root@k8s-control-plane:/# cat /etc/crictl.yaml 
runtime-endpoint: unix:///run/containerd/containerd.sock
```


On the worker node, there are also pods:
```console
$ docker exec -it k8s-worker2 crictl pods
POD ID              CREATED             STATE               NAME                              NAMESPACE           ATTEMPT             RUNTIME
cfcc173e2c951       26 minutes ago      Ready               dep-test-taint-69d9bf7844-c8bn7   default             0                   (default)
55fb56084cc50       26 minutes ago      Ready               dep-test-taint-69d9bf7844-wwbc4   default             0                   (default)
5bd9df99eb5f3       2 hours ago         Ready               kube-proxy-sw7fd                  kube-system         0                   (default)
bdb32481e0cb4       2 hours ago         Ready               kindnet-lktrn                     kube-system         0                   (default)
```

With crictl, you can also list containers:

```console
root@k8s-worker2:/# crictl ps
CONTAINER           IMAGE               CREATED             STATE               NAME                ATTEMPT             POD ID              POD
a15d10b841dc8       a72860cb95fd5       35 minutes ago      Running             nginx               0                   cfcc173e2c951       dep-test-taint-69d9bf7844-c8bn7
67ce2fb36ba24       a72860cb95fd5       35 minutes ago      Running             nginx               0                   55fb56084cc50       dep-test-taint-69d9bf7844-wwbc4
76ccc64caeb8b       4950bb10b3f87       2 hours ago         Running             kindnet-cni         0                   bdb32481e0cb4       kindnet-lktrn
804239075746c       c4e12eee82f28       2 hours ago         Running             kube-proxy          0                   5bd9df99eb5f3       kube-proxy-sw7fd
```

and go into each one of them:
```console
root@k8s-worker2:/# crictl exec -i -t 804239075746c sh
```

Or retrieve logs from containers
```console
# GET ALL THE LOG
root@k8s-worker2:/# crictl log 804239075746c
# GET ONLY THE 5 LAST LINES
root@k8s-worker2:/# crictl log --tail=5 804239075746c
```

To pull an image
```console
root@k8s-worker2:/# crictl pull busybox
```

```console
root@k8s-worker2:/# echo '{
    "metadata": {
        "name": "nginx-sandbox",
        "namespace": "default",
        "attempt": 1,
        "uid": "hdishd83djaidwnduwk28bcsb"
    },
    "log_directory": "/tmp",
    "linux": {}
}' > pod.json
root@k8s-worker2:/# crictl runp pod.json

root@k8s-worker2:/# echo '{
    "metadata": {
        "name": "nginx-sandbox",
        "namespace": "default",
        "attempt": 1,
        "uid": "hdishd83djaidwnduwk28bcsb"
    },
    "log_directory": "/tmp",
    "linux": {}
}' > pod.json
```

