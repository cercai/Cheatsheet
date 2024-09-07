
# Certificates

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

Or generate the private key and the csr in one commande:
```console
$ openssl req -new -key cercai.key -subj "/CN=cercai" -out cercai.csr
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
```


```console
$ cat cercai.csr | base64 -w0
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZqQ0NBVDRDQVFBd0VURVBNQTBHQTFVRUF3d0dZMlZ5WTJGcE1JSUJJakFOQmdrcWhraUc5dzBCQVFFRgpBQU9DQVE4QU1JSUJDZ0tDQVFFQW5UZmxTWXBjQ0lYbFA3WHdXYk5iNXRZaUMwSkF6UTJmZkZwcEkyVzBLdTVZCkJ1MXM2UmpDdGk1LzZYRW5XMHIwSkhOdVhEVlJUa1hxYWZlMlVaRXNORWtNakhXOCtKdTNyeXNDNitlYVo4bjgKMGlpMG5KVTd0S2cxc2x5WTJyUUFXb05uMStxYXBTL044aC9tdkh0Tmhpb1JTV0hJNWluVnZkd0ZwR2d3WTkzbAp3aWIxQ25GLzRDcVJQY3pkTkV5M2RqcmJydjY5Q05CcUxJakZza01sNEtyRG1GUmozYUlldUZoU0hGdHNpamZkCnpxZkZIeWZaQU9uVmJRbkVVWUt1TURXN2xFV3poYitVTHRVMUVMbGd5MnM2THlURjFJTlFYWmJEOTFFa3k1SDgKSEQ2WWJlTmtNYXlqcG8wTlYweWpNYjJ1UWwrRDlNWmlmNmRhMlpRc1R3SURBUUFCb0FBd0RRWUpLb1pJaHZjTgpBUUVMQlFBRGdnRUJBQ2hzL1RjVXN6a1Y2SHN6YlE4N2twVHBFdks1THlGdkJpby9rVjBvOTZpZnZGOS9ZRHUvCmRmeGRzb0Q2ZjVEZkw4ZFBMTk9HYXFseUFMQXlCNjBwSjJjS09uWjlzc3ZTNDh6OE40V2V3dUNxdUVLdVo4SWEKUVNEVTRoWVZuWjJjY051Ly9kMUo0d2FIcE9BcHd0eWJMQ2cxa294cXE4eGwzRmp5RlRra0hmeEgzeVR3U2hoaQpRNUwrMm9wQWxMbXh1d3VoQkFySnlEQkYvNGpKVEt5QVhSNjZVUytXTFF5dW9hd25ZM1liK1g5RmRYaFZBUnhVCm9TdnJLZUtHOXl1d0JzTTMzOEVPbUFETFYrYmRJVXJXb1F3enZtZmJpZms2dVl0LzYzdXBreTFHQUJMbElnOGgKNFY3S2FqcEVxTU5XeitudWJwcTNWQWN3cDlnR3pLdVVNNjg9Ci0tLS0tRU5EIENFUlRJRklDQVRFIFJFUVVFU1QtLS0tLQo=
```

Retrieve the template for doing a csr [here](https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/) and create a file named cercai-csr.yaml

```sh
$ cat << EOF > cercai-csr.yaml
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: myuser
spec:
  request: myencodedcertificate
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400  # one day
  usages:
  - client auth
EOF
```

Copy the csr in base64 in the cercai-csr.yaml file.
```sh
$ sed -i "s/myuser/cercai/" cercai-csr.yaml
$ sed -i "s/myencodedcertificate/$(cat cercai.csr | base64 -w0)/" cercai-csr.yaml
```

Create the csr on the cluster
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






