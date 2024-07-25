```sh
$ cat  /etc/sysctl.d/kubernetes.conf
    net.bridge.bridge-nf-call-ip6tables = 1
    net.bridge.bridge-nf-call-iptables = 1
    net.ipv4.ip_forward = 1
```

List pods dans le control-plane
```sh
kubectl get pods --all-namespaces
NAMESPACE     NAME                                        READY   STATUS    RESTARTS           AGE
default       ingress-nginx-controller-6dfcb8658d-jcd4d   1/1     Running   1 (8m26s ago)      40d
kube-system   cilium-operator-788c4f69bc-2frtf            1/1     Running   6652 (3d20h ago)   95d
kube-system   cilium-operator-788c4f69bc-gfxlf            1/1     Running   4 (8m30s ago)      95d
kube-system   cilium-td76c                                1/1     Running   4 (8m30s ago)      95d
kube-system   cilium-x7t4x                                1/1     Running   6945 (3d20h ago)   95d
kube-system   coredns-5dd5756b68-mhj8j                    1/1     Running   4 (8m30s ago)      95d
kube-system   coredns-5dd5756b68-wq6cq                    1/1     Running   4 (8m30s ago)      95d
kube-system   etcd-instance-master                        1/1     Running   4 (8m30s ago)      95d
kube-system   kube-apiserver-instance-master              1/1     Running   4 (8m30s ago)      95d
kube-system   kube-controller-manager-instance-master     1/1     Running   4 (8m30s ago)      95d
kube-system   kube-proxy-6vclt                            1/1     Running   4 (8m30s ago)      95d
kube-system   kube-proxy-8dtbp                            1/1     Running   6 (8m26s ago)      95d
kube-system   kube-scheduler-instance-master              1/1     Running   4 (8m30s ago)      95d
```

# Create pod
kubectl run nginx --image nginx

# Créer un replicationController

Il n'existe pas de commande, il faut faire un `kubectl apply -f <file>`
```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-rc
spec:
  replicas: 3
  selector:
    app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21.6
        ports:
        - containerPort: 80
```


# Create deployment
kubectl create deployment nginx --image=nginx


```yaml limitrange.yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: low-resource-range
spec:
  limits:
  - default:
      cpu: 1
      memory: 500Mi
    defaultRequest:
      cpu: 0.5
      memory: 100Mi
    type: Container
```


k create namespace limited-ns
k -n limited-ns create -f limitrange.yaml
k get limitrange -n limited-ns


k get events
k expose --help
kubectl expose deployment/nginx
kubectl replace -f first.yaml --force

kubectl get deploy,pod
sudo tcpdump
sudo tcpdump -i <interface>


List the interfaces
```sh
ip address show
```
For a brief answer, use `-br` and colorize with `-c`.<br>
**a** stands for address.
```sh
ip -br -c a
```

To see the trafic on the interface use tcpdump
```sh
sudo tcpdump -i cilium_vxlan
```



ip routes pour voir les interfaces

