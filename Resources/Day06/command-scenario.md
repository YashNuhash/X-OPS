# 🔧 Kubernetes Kind Cluster Setup & Management

## ✅ Create First Kind Cluster: `cluster1`

```powershell
kind create cluster --name cluster1
```

**Output:**

```
Creating cluster "cluster1" ...
 ✓ Ensuring node image (kindest/node:v1.33.1) 🖼
 ✓ Preparing nodes 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
Set kubectl context to "kind-cluster1"

You can now use your cluster with:
kubectl cluster-info --context kind-cluster1
```

### 🔍 Check Cluster Info

```powershell
kubectl cluster-info --context kind-cluster1
```

**Output:**

```
Kubernetes control plane is running at https://127.0.0.1:55752
CoreDNS is running at https://127.0.0.1:55752/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

---

## 📝 Prepare Config File

```powershell
notepad config.yaml
```

(Create your custom `config.yaml` in Notepad and save.)

---

## ✅ Create Multi-node Cluster: `cluster2`

```powershell
kind create cluster --name cluster2 --config config.yaml
```

**Output:**

```
Creating cluster "cluster2" ...
 ✓ Ensuring node image (kindest/node:v1.33.1) 🖼
 ✓ Preparing nodes 📦 📦 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
 ✓ Joining worker nodes 🚜
Set kubectl context to "kind-cluster2"
```

---

## 📄 View All Nodes

```powershell
kubectl get nodes
```

**Output for `cluster2`:**

```
NAME                     STATUS   ROLES           AGE     VERSION
cluster2-control-plane   Ready    control-plane   2m16s   v1.33.1
cluster2-worker          Ready    <none>          2m4s    v1.33.1
cluster2-worker2         Ready    <none>          2m4s    v1.33.1
```

---

## 🔁 Check Available Contexts

```powershell
kubectl config get-contexts
```

**Output:**

```
CURRENT   NAME             CLUSTER          AUTHINFO         NAMESPACE
          docker-desktop   docker-desktop   docker-desktop
          kind-cluster1    kind-cluster1    kind-cluster1
*         kind-cluster2    kind-cluster2    kind-cluster2
```

---

## 🔄 Switch to `cluster1` Context

```powershell
kubectl config use-context kind-cluster1
```

**Then check nodes:**

```powershell
kubectl get nodes
```

**Output:**

```
NAME                     STATUS   ROLES           AGE   VERSION
cluster1-control-plane   Ready    control-plane   21m   v1.33.1
```

---

## 🔄 Switch back to `cluster2` Context

```powershell
kubectl config use-context kind-cluster2
```

**Then check nodes:**

```powershell
kubectl get nodes
```

**Output:**

```
NAME                     STATUS   ROLES           AGE     VERSION
cluster2-control-plane   Ready    control-plane   8m36s   v1.33.1
cluster2-worker          Ready    <none>          8m24s   v1.33.1
cluster2-worker2         Ready    <none>          8m24s   v1.33.1
```

---

