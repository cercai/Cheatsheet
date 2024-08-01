# Kind

Kind: Kubernetes in Docker, is a tool designed for running local Kubernetes clusters
using Docker container "nodes". 

## Create clusters

To create a cluster: `kind create cluster --name <cluster-name> --config <config-file>`
```sh
# Create a cluster with a default name: kind
$ kind create cluster
# Cluster with a specific name
$ kind create cluster --name kind-cluster
# Create a cluster using a configuration file
$ kind create cluster --name cluster-1 --config config.yaml
```

The configuration file can list the nodes and their roles
```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: control-plane
- role: worker
- role: worker
- role: worker
```

An image could also be specified:
```yaml
- role: control-plane
  image: kindest/node:v1.30.2@sha256:ecfe5841b9bee4fe9690f49c118c33629fa345e3350a0c67a5a34482a99d6bba
- role: worker
  image: kindest/node:v1.30.2@sha256:ecfe5841b9bee4fe9690f49c118c33629fa345e3350a0c67a5a34482a99d6bba
```

## List the clusters, the nodes

List the existing clusters:
```sh
$ kind get clusters
kind
kind-cluster
cluster-1
```

List the nodes: `kind get nodes --name <cluster-name>`
```sh
$ kind get nodes --name cluster-1
cluster-1-control-plane
cluster-1-worker2
cluster-1-worker
```

## Remove clusters

Remove cluster: `kind delete cluster --name <cluster-name>`
```sh
$ kind delete cluster --name kind
```

## Images

Load an image into your cluster with the command `kind load docker-image <image>`
```sh
$ kind load docker-image nginx
```

```sh
$ docker exec -it <controle-plane-cont> crictl images
```

## Cluster configuration


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

# Cluster's logs

Get cluster's logs
```sh
$ kind export logs --name kind-cluster
Exporting logs for cluster "kind-cluster" to:
/tmp/3583915737
```
