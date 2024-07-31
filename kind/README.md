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
kubectl config use-context kind-kind-cluster
```

```sh
kubectl config get-contexts
```




