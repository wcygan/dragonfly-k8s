# DragonflyDB on Kubernetes

## Installation

Run the following commands to install the DragonflyDB operator and deploy a cache:

```bash
skaffold run -p bootstrap 
skaffold run
```

Check for the pods to be ready:

```bash
kubectl get pods -n dragonfly-operator-system
NAME                                                     READY   STATUS    RESTARTS   AGE
dragonfly-operator-controller-manager-547ddd69d9-jlnsr   2/2     Running   0          77s
example-cache-0                                          1/1     Running   0          53s
```

## Example Usage

Use the following commands to interact with the cache:

```bash
kubectl exec -it example-cache-0 -n dragonfly-operator-system -- redis-cli set foo bar
OK

kubectl exec -it example-cache-0 -n dragonfly-operator-system -- redis-cli get foo
"bar"
```

You can check that data was replicated to another instance by running the following command:

```bash
kubectl exec -it example-cache-1 -n dragonfly-operator-system -- redis-cli get foo
"bar"
```

In the above command, we checked `example-cache-1` for the key `foo` and found that it was replicated from `example-cache-0`.