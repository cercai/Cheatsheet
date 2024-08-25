
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

Generate public and private keys
```console
$ ssh-keygen -t rsa -b 4096 -f cercai
$ ls
cercai cercai.pub
# CONVERT INTO PEM FORMAT
$ ssh-keygen -p -m PEM -f cercai
Enter old passphrase: 
Enter new passphrase (empty for no passphrase): 
Enter same passphrase again:
```

Another way to do it is using openssl

```console
$ openssl genrsa -out cercai
```

Create Certificate Signing Request
```console
$ openssl req -new -key cercai -subj "/CN=cercai" -out cercai.csr
Enter pass phrase for cercai:
```

```console
$ cat cercai.csr | base64 -w0
LS0.....
```
Copy the csr in base64 in the cercai-csr.yaml file.
```console
$ vi cercai-csr.yaml
```

```console
$ kubectl apply -f cercai-csr.yaml
$ kubectl get csr
NAME     AGE   SIGNERNAME                            REQUESTOR          REQUESTEDDURATION   CONDITION
cercai   13s   kubernetes.io/kube-apiserver-client   kubernetes-admin   24h                 Pending
```

Approve (or deny) the csr
```console
$ kubectl certificate approve cercai
```

Verify the csr has been approved in the CONDITION column.
```console
$ kubectl get csr                   
NAME     AGE     SIGNERNAME                            REQUESTOR          REQUESTEDDURATION   CONDITION
cercai   6m52s   kubernetes.io/kube-apiserver-client   kubernetes-admin   24h                 Approved,Issued
```

Fetch the certificate from the csr
```console
$ kubectl get csr cercai -o jsonpath='{.status.certificate}' > cercai.crt
```

Verify that 'CN=cercai' is present in the data of the certificate
```console
cat cercai.crt | base64 -d | openssl x509 -noout -text
```




