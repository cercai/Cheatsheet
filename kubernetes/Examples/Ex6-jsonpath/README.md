
# JSONPath

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



```console
# GET FIRST POD'S NAME
$ kubectl get pod -o jsonpath='{.items[0].metadata.name}'
# GET PODS' NAME ON ONE LINE
$ kubectl get pod -o jsonpath='{.items[*].metadata.name}'
# GET PODS' NAME ON ONE COLUMN
$ kubectl get pod -o jsonpath='{range .items[*]}{.metadata.name}{"\n"}{end}'
```

Get several data with jsonpath
```console
$ kubectl get pod -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.podIP}{"\n"}{end}'
nginx-dep-649bd465f4-ssbpw      10.244.2.2
nginx-depl-6b77bdbbdc-75v9v     10.244.1.2
```

Get several data with custom-columns
```console
$ kubectl get pods -o custom-columns=POD_NAME:.metadata.name,IP:.status.podIP
POD_NAME                      IP
nginx-dep-649bd465f4-ssbpw    10.244.2.2
nginx-depl-6b77bdbbdc-75v9v   10.244.1.2
```