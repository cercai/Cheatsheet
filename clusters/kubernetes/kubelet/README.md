# Kubelet

## What is Kubelet ?

Kubelet is the primary node agent in a Kubernetes cluster. It:
- Runs on every node in the cluster.
- Registers the node with the Kubernetes control plane.
- Ensures containers described in PodSpecs are running and healthy.
- Monitors the state of pods and reports to the control plane.

Kubelet interacts with:
- The container runtime (like Docker, containerd).
- The Kubernetes API server.

## The service Kubelet

Rentrer dans n'importe quel Node et regarder les services:
```console
systemctl list-unit-files --type=service --state=enabled
UNIT FILE                STATE   PRESET 
containerd.service       enabled enabled
kubelet.service          enabled enabled
open-iscsi.service       enabled enabled
undo-mount-hacks.service enabled enabled
```

- containerd is the container runtime
- kubelet is the service we focus on
- open-iscsi is a service that provides iSCSI support (Internet Small Computer System Interface) and is used by the storage plugin in Kubernetes
- undo-mount-hacks is a hack to unmount /proc/cgroups

To see the logs of each one of these services, just type `journalctl -fu <service>`.

## Kubelet configuration

The most important configuration file for kubelet is this one: `/etc/kubernetes/kubelet.conf`
From this file, we can found the crt of the certificat authority, the api-server hostname and the location of the cert/key used by kubelet to discuss with the api-server. In my case, it can be found here /var/lib/kubelet/pki/kubelet-client-current.pem
```console
# curl -k https://demo-control-plane:6443
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "forbidden: User \"system:anonymous\" cannot get path \"/\"",
  "reason": "Forbidden",
  "details": {},
  "code": 403
}

# cat /var/lib/kubelet/pki/kubelet-client-current.pem
-----BEGIN CERTIFICATE-----
MIICZTCCAU2gAwIBAgIRAKHVgDqHkURjC4R+ff7//xAwDQYJKoZIhvcNAQELBQAw
FTETMBEGA1UEAxMKa3ViZXJuZXRlczAeFw0yNTA1MTExMjI1NTJaFw0yNjA1MTEx
MjI1NTJaMDoxFTATBgNVBAoTDHN5c3RlbTpub2RlczEhMB8GA1UEAxMYc3lzdGVt
Om5vZGU6ZGVtby13b3JrZXIyMFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEDlBP
XrmZZE5rJOeef+ttk3j7D1WW8ZKEtKx8T0t5awAVJlraOzg5nBVOAgoiY5BmZcxL
N0lxowLuKYFl3ABHuKNWMFQwDgYDVR0PAQH/BAQDAgeAMBMGA1UdJQQMMAoGCCsG
AQUFBwMCMAwGA1UdEwEB/wQCMAAwHwYDVR0jBBgwFoAUTT/Tdc5N6Y5mx4sEQG/T
YJidFh0wDQYJKoZIhvcNAQELBQADggEBAE9YRpQkCSTVq9vhXU1j23X4EtDvnlI8
tq8OLml2FCtsgFUFP12xKXp9WJR+TDfu0iC8Pe3izChX/zn8Ye52I9U8TdYQMGTH
cJOuxeMCpdWEKsC9cEqnlc0O8G2OpJj1hSUlxAmFFioiGAScNvFX/5zGBAjqrRD3
hOKYPrLIIE1VxRTxCYMES4640VXpn/s+P2p8ttpyNa/O9rab5AratpQq4oXmpQlA
8fTkONseiH+YVM78sZCGSsix9hZvgWgdrG0CWtzO/g+Zuf/CyrThg72WK/WgDqeR
HoSPcawuayUULagvbdDAPkh8P2uFYCW/jtXmniAo/r+ErOyCZl/ls/A=
-----END CERTIFICATE-----
-----BEGIN EC PRIVATE KEY-----
MHcCAQEEIBbBFUBf+YrZB+Lzenw3M8Y0uxL9DDjCXaYgsWNyOWtmoAoGCCqGSM49
AwEHoUQDQgAEDlBPXrmZZE5rJOeef+ttk3j7D1WW8ZKEtKx8T0t5awAVJlraOzg5
nBVOAgoiY5BmZcxLN0lxowLuKYFl3ABHuA==
-----END EC PRIVATE KEY-----
```

With openssl, the user can read the certificate or make sure the certificate corresponds to the key
```console
# Reading the certificate
openssl x509 -in /var/lib/kubelet/pki/kubelet-client-current.pem -text -noout
# Verify the key/cert match
# First, retrieve the public key from the private key
openssl ec -in key.tmp -pubout -out pub.tmp
# Then retrieve the public key from the certificate
openssl x509 -in crt.tmp -pubkey -noout > pub.tmp2
# Then compare both public keys
```

Remake the curl request but with the certificates and keys
```console
curl --cert /var/lib/kubelet/pki/kubelet-client-current.pem \
     --key /var/lib/kubelet/pki/kubelet-client-current.pem \
     --cacert /etc/kubernetes/pki/ca.crt \
     https://demo-control-plane:6443/api
{
  "kind": "APIVersions",
  "versions": [
    "v1"
  ],
  "serverAddressByClientCIDRs": [
    {
      "clientCIDR": "0.0.0.0/0",
      "serverAddress": "172.18.0.2:6443"
    }
  ]
}
```

A configuration file is found here: `/var/lib/kubelet/config.yaml`. Some info like the location of the certificate of the certificate authority or the DNS cluster IP can be found in it.




