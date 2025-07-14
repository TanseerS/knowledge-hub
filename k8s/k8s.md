# **Kubernetes (k8s)**

## Ways to create a pod:
1. **Imperative**: directly using kubectl
```bash
kubectl run my-pod --image=my-image --restart=Never
```
2. **Declarative**: using a config file (json/yml)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: my-image
```
---
## Replication Controller
- Ensures a specified number of pod replicas are running at all times.
- If a pod fails, the controller creates a new one to maintain the desired state.
```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: my-rc
spec:
  replicas: 3
  selector:
    app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: my-image
```
---