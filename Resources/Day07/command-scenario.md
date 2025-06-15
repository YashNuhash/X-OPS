
```bash
# Check current kubectl contexts
kubectl config get-contexts
```

**Output:**

```
CURRENT   NAME            CLUSTER         AUTHINFO        NAMESPACE
*         kind-cluster1   kind-cluster1   kind-cluster1
          kind-cluster2   kind-cluster2   kind-cluster2
```

```bash
# Create an NGINX pod
kubectl run nginx-pod --image=nginx:latest
```

**Output:**

```
pod/nginx-pod created
```

```bash
# Check if node is ready
kubectl get nodes
```

**Output:**

```
NAME                     STATUS   ROLES           AGE     VERSION
cluster1-control-plane   Ready    control-plane   2m43s   v1.33.1
```

```bash
# List running pods
kubectl get pods
```

**Output:**

```
NAME        READY   STATUS    RESTARTS   AGE
nginx-pod   1/1     Running   0          43s
```

Here's the entire sequence of your Kubernetes commands, refined in **Markdown** format for clean documentation:

---

## 📘 Inspect Pod Resource Structure

```bash
kubectl explain pod
```

**Output:**

```
KIND:       Pod
VERSION:    v1

DESCRIPTION:
    Pod is a collection of containers that can run on a host...

FIELDS:
  apiVersion  <string>     # Defines the versioned schema
  kind        <string>     # Represents the REST resource type
  metadata    <ObjectMeta> # Standard object metadata
  spec        <PodSpec>    # Desired behavior of the pod
  status      <PodStatus>  # Observed status of the pod (read-only)
```

---

## 📝 Create a Pod from YAML

```bash
notepad pod.yaml
```

#### pod.yaml file : 

```YAML
# This is a sample pod yaml

apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    env: demo
    type: frontend
spec:
  containers:
  - name: nginx-container
    image: nginx
    ports:
    - containerPort: 80
```
And then Typing this hit enter: 

```bash
kubectl create -f pod.yaml
```

**Error (if pod already exists):**

```
Error from server (AlreadyExists): error when creating "pod.yaml": pods "nginx-pod" already exists
```

---

## 🗑 Delete Existing Pod

```bash
kubectl delete pod nginx-pod
```

**Output:**

```
pod "nginx-pod" deleted
```

---

## ✅ Recreate the Pod

```bash
kubectl create -f pod.yaml
```

**Output:**

```
pod/nginx-pod created
```

---

## 📦 List Pods

```bash
kubectl get pods
```

**Output:**

```
NAME        READY   STATUS    RESTARTS   AGE
nginx-pod   1/1     Running   0          12s
```

---

## 🔍 Describe Pod

```bash
kubectl describe pod nginx-pod
```

**Highlights from Output:**

* **Node:** `cluster1-control-plane`
* **Labels:** `env=demo`, `type=frontend`
* **Status:** `Running`
* **Pod IP:** `10.244.0.6`
* **Container:** `nginx-container` running image `nginx`
* **Events:**

  * Successfully assigned
  * Pulled image
  * Created and Started container

---

## 🌐 View Pod with Wide Output

```bash
kubectl get pods -o wide
```

**Output:**

```
NAME        READY   STATUS    RESTARTS   AGE     IP           NODE                     NOMINATED NODE   READINESS GATES
nginx-pod   1/1     Running   0          9m27s   10.244.0.6   cluster1-control-plane   <none>           <none>
```

---

## 🧱 View Node Info

```bash
kubectl get nodes
```

**Output:**

```
NAME                     STATUS   ROLES           AGE   VERSION
cluster1-control-plane   Ready    control-plane   26m   v1.33.1
```

---

## 📊 View Node Info with Details

```bash
kubectl get nodes -o wide
```

**Output:**

```
NAME                     STATUS   ROLES           AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                         KERNEL-VERSION                     CONTAINER-RUNTIME
cluster1-control-plane   Ready    control-plane   26m   v1.33.1   172.18.0.2    <none>        Debian GNU/Linux 12 (bookworm)   6.6.87.1-microsoft-standard-WSL2   containerd://2.1.1
```

---

