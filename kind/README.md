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


