# ETCD

Etcd is a distributed key-value store designed to save critical data for distributed systems.

In a single-node (e.g. Kind or Minikube), etcd runs as a static pod  and its configuration file might be found in the controle plane pod in `/etc/kubernetes/manifests/etcd.yaml` whereas in a production cluster, etcd typically runs as a dedicated cluster of 3 or more nodes.

The data is stored in `/var/lib/etcd/member` with Snapshot (.snap) and Write-Ahead Log (.wal) files.
```console
kubectl exec -n kube-system -it etcd-demo-control-plane -- sh -c "cd /var/lib/etcd/member/ && echo *"
snap wal
```

## PKI certificates and keys
The pki directory contains certificate and key of Certificate Authority, the etcd server when contacted by peers, or by api-server.
```console
kubectl exec -n kube-system -it etcd-demo-control-plane -- sh -c "echo /etc/kubernetes/pki/etcd/*"
/etc/kubernetes/pki/etcd/ca.crt /etc/kubernetes/pki/etcd/ca.key /etc/kubernetes/pki/etcd/healthcheck-client.crt /etc/kubernetes/pki/etcd/healthcheck-client.key /etc/kubernetes/pki/etcd/peer.crt /etc/kubernetes/pki/etcd/peer.key /etc/kubernetes/pki/etcd/server.crt /etc/kubernetes/pki/etcd/server.key
```

## Get data from database
Get all the keys
```cmd
etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt \
        --cert=/etc/kubernetes/pki/etcd/healthcheck-client.crt \
        --key=/etc/kubernetes/pki/etcd/healthcheck-client.key \
        get "" --prefix --keys-only
```

Get a specific value, such as the namespaces in the cluster

```cmd
$ etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt \
        --cert=/etc/kubernetes/pki/etcd/healthcheck-client.crt \
        --key=/etc/kubernetes/pki/etcd/healthcheck-client.key \
	get /registry/namespaces/ --prefix --keys-only

/registry/namespaces/default
/registry/namespaces/kube-node-lease
/registry/namespaces/kube-public
/registry/namespaces/kube-system
/registry/namespaces/local-path-storage
```

## Save a snapshot

```console
etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt \
        --cert=/etc/kubernetes/pki/etcd/healthcheck-client.crt \
        --key=/etc/kubernetes/pki/etcd/healthcheck-client.key \
	snapshot save <snapshot's name>
```

## Restore a snapshot

```console
etcdctl --cacert=/etc/kubernetes/pki/etcd/ca.crt \
        --cert=/etc/kubernetes/pki/etcd/healthcheck-client.crt \
        --key=/etc/kubernetes/pki/etcd/healthcheck-client.key \
        snapshot save <snapshot's path>
```


