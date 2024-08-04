
# Expose

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
$ kubectl apply -f deploy-nginx.yaml          
deployment.apps/nginx-depl created

$ kubectl expose deployment/nginx-depl
error: couldn't find port via --port flag or introspection
```


```console
$ kubectl apply -f deploy-nginx-with-port.yaml 
deployment.apps/nginx-depl configured

$ kubectl get services                                                                   
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   18h

$ kubectl expose deployment/nginx-depl                  
service/nginx-depl exposed

$ kubectl get services                
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP   18h
nginx-depl   ClusterIP   10.96.245.172   <none>        80/TCP    7s
```

## Request the service

```console
$ kubectl get ep      
NAME         ENDPOINTS         AGE
kubernetes   172.18.0.4:6443   18h
nginx-depl   10.244.1.4:80     3m59s
```


```console
# ENTER THE MASTER OR THE WORKER NODE/CONTAINER
$ docker exec -it <container-name> bash

$ curl 10.244.1.4:80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...


curl 10.96.245.172:80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...
```