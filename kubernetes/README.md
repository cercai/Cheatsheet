# Nodes

List the nodes
```sh
kubectl get nodes -o wide
```
Describe them
```sh
kubectl describe nodes
kubectl describe nodes | grep -i taint
```


## Nodes Taints
Taints allow a node to repel a set of pods.<br>
Add a taint to the node `node1`.
```sh
kubectl taint nodes node1 key1=value1:NoSchedule
```
To remove the taint from the command line above, add the '-' at the end.
```sh
kubectl taint nodes node1 key1=value1:NoSchedule-
```


# Deployments

List deployments
```sh
kubectl get deployment -o wide
```

## Create a deployment

Create a simple deployment
```sh
# Simple nginx deployment
kubectl create deployment nginx --image nginx
# Create yaml file
kubectl create deployment nginx --image nginx --dry-run=client -o yaml > nginx-deploy.yaml
# Add more replicas
kubectl create deployment nginx --image nginx --replicas=3
# Create a deploy from a yaml file
kubectl apply -f nginx-deploy.yaml
```

## Expose ports
In case you can't expose the ports
```sh
k expose deployment/nginx
    error: couldn't find port via --port flag or introspection
```

You might need to define a containerPort in the deployment's specification.
You can edit the deployment with the `kubectl edit deployment/nginx` command 
```yaml
containers:
- image: nginx
  imagePullPolicy: Always
  name: nginx
  # Add the following lines
  ports:
  - containerPort: 80
  protocol: TCP
```
Then redo the`expose` command
```sh
kubectl expose deployment/nginx
    service/nginx exposed
```

A service and an endpoint have been created.
```sh
kubectl get svc
kubectl get ep
```