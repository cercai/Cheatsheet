
# Kubelet

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

Enter into the worker or control plane node and check the kubelet status

```console
$ service kubelet status
# OR
$ systemctl status kubelet
```

In case of bug, read the journal
```console
$ journalctl -u kubelet
```

Update the service configuration if required

```console
$ sudo vi /etc/systemd/system/kubelet.service
$ sudo systemctl daemon-reload
$ sudo systemctl restart kubelet
```

