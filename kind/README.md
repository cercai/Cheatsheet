# KIND

To create a cluster: `kind create cluster --name <cluster-name>`
```sh
$ kind create cluster --name kind-cluster
$ kind create cluster     # default name: kind
```

List clusters:
```sh
$ kind get clusters
kind
kind-cluster
```

Remove cluster: `kind delete cluster --name <cluster-name>
```sh
$ kind delete cluster --name kind
```


Get nodes: `kind get nodes --name <cluster-name>`
```sh
$ kind get nodes --name kind-cluster
kind-cluster-control-plane
```

Get cluster configuration file `kind get kubeconfig --name <cluster-name>`
```yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1...
...
```


```sh
$ kubectl cluster-info --context kind-kind-cluster
Kubernetes control plane is running at https://127.0.0.1:43297
CoreDNS is running at https://127.0.0.1:43297/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

```sh
$ kubectl config use-context kind-kind-cluster
```

```sh
$ kubectl config get-contexts
```

Load image into your cluster
```sh
$ kind load docker-image my-custom-image-0
```


```sh
$ docker exec -it <controle-plane-cont> crictl images
```

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: control-plane
- role: control-plane
- role: worker
- role: worker
```

Get cluster's logs
```sh
$ kind export logs --name kind-cluster
Exporting logs for cluster "kind-cluster" to:
/tmp/3583915737
```

