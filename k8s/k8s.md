# **Kubernetes (k8s)**

## Ways to create a pod:
1. Imperative: directly using kubectl
```bash
kubectl run my-pod --image=my-image --restart=Never
```
2. Declarative: using a config file (json/yml)
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