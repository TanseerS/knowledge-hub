# **Kubernetes (k8s)**

## What is Kubernetes?
Kubernetes is a **container orchestration platform** designed to automate the deployment, scaling, and management of containerized applications.

---

## Problems Solved by Kubernetes
- Limited to single-host environments → enables multi-node deployments
- Lack of auto-healing → detects and replaces failed containers
- No auto-scaling → automatically scales based on demand
- Missing enterprise features → provides load balancer, firewall, DNS, etc.

---

## Monolithic vs Microservices Applications
- **Monolithic**: Large, single-unit applications, harder to scale and update.
- **Microservices**: Smaller, independent services that communicate over the network, easier to scale, test, and maintain.

---

## Kubernetes Architecture

### Cluster
A **cluster** consists of multiple **nodes** (servers), divided into:
- **Master nodes (Control Plane)**
- **Worker nodes (Data Plane)**

---

### Master Node (Control Plane)
Responsible for managing the cluster:
- **API Server**: Central control, handles all communication between master and workers.
- **Scheduler**: Assigns containers/pods to appropriate nodes.
- **etcd**: Distributed key-value store for all cluster data (configs, secrets, etc.).
- **Controller Manager**: Ensures the actual state matches the desired state.
- **Cloud Controller Manager**: Integrates with cloud provider APIs to manage cluster lifecycle.

---

### Worker Node (Data Plane)
Executes the workload (containers):
- **Pod**: Smallest deployable unit in Kubernetes, wraps one or more containers.
- **Kubelet**: Agent that runs on each node; ensures containers are running in a pod.
- **Kube Proxy**: Handles networking and load-balancing across pods.
- **Container Runtime**: Executes containers (e.g., `containerd`, `CRI-O`). Docker's `dockershim` is deprecated since 2022.

---

### Networking in Kubernetes
- **CNI (Container Network Interface)**: Enables networking between pods, services, master, and worker nodes.
- **kubectl**: CLI to interact with the Kubernetes cluster — used for deploying apps, debugging, and managing resources.

---

## Ways to Create a Cluster
1. **Kubeadm** – Production-ready, manually installs K8s on multiple instances.
2. **Minikube** – Local single-node cluster (ideal for learning/testing).
3. **KIND (Kubernetes IN Docker)** – Runs K8s clusters in Docker containers.
4. **EKS / AKS / GKE** – Managed services from AWS, Azure, GCP respectively.

---

## YAML in Kubernetes
- Kubernetes manifests (like pods, deployments) are written in `.yml` files.
- Think of them as **declarative Docker run commands**.

---

## Pods, Deployments, and ReplicaSets

### Pods
- Smallest unit in K8s
- Run one or more containers
- **Do not** auto-heal or auto-scale on their own

### 🛠 Deployments
- Higher-level abstraction over pods
- Automatically manages ReplicaSets
- Supports auto-healing and scaling

### ReplicaSets
- Kubernetes controller that ensures a specified number of pod replicas are running at any time.

### Production Tip
> Always create **Deployments** instead of raw Pods. The deployment handles replica sets which then manage pods — and ensure self-healing and desired state maintenance.

---

## Services in Kubernetes

### Why Use Services?
- Deployments handle self-healing, but IPs of pods can change.
- **Services** provide a consistent way to expose and access pods using **labels**, not IPs.

### How Services Work
- Use `kube-proxy` to route traffic.
- Match pods using **labels** instead of dynamic IPs.
- Acts like a built-in load balancer.

### Service Types
- **ClusterIP**: Default; internal-only access within the cluster.
- **NodePort**: Exposes app on a static port on each node; accessible within VPC/network.
- **LoadBalancer**: Provisions external IP (usually via cloud provider) for public access.

---

## Ingress in Kubernetes

### Problems with Services
- Traditional enterprise apps used powerful load balancers (like NGINX) with:
  - Sticky sessions
  - Weighted routing
  - Access control (whitelisting/blacklisting)
  - URL/path-based routing
- In Kubernetes:
  - Each service may require its own LoadBalancer → leads to **many public IPs** → **high cloud costs**

### Ingress to the Rescue
- Acts as a **reverse proxy** and **layer 7 load balancer**
- Manages external access to services
- Routes based on **hostnames or paths**
- Consolidates many services behind a **single IP**


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

## Replication Controller
- Ensures a specified number of pod replicas are running at all times.
- If a pod fails, the controller creates a new one to maintain the desired state.
- Can also create pods in new nodes if the traffic increases.
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

