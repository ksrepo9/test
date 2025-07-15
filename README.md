# Kubernetes Scenario-Based Pod Interview Questions (2025)

A collection of scenario-based interview questions focused on Kubernetes Pods, formatted for technical documentation. Updated July 2025.

## Table of Contents
1. [Multi-Container Pod Communication](#1-multi-container-pod-communication)  
2. [Pod Scheduling on GPU Nodes](#2-pod-scheduling-on-gpu-nodes)  
3. [Persistent Storage for Stateful Pods](#3-persistent-storage-for-stateful-pods)  
4. [Liveness vs Readiness Probes](#4-liveness-vs-readiness-probes)  
5. [Init Containers for Pre-Start Tasks](#5-init-containers-for-pre-start-tasks)  
6. [Resource Limits and OOMKills](#6-resource-limits-and-oomkills)  
7. [Pod Networking Issues](#7-pod-networking-issues)  
8. [Pod Security Context](#8-pod-security-context)  

---

### 1. Multi-Container Pod Communication <a name="1-multi-container-pod-communication"></a>
**Scenario**:  
A Pod contains a primary application container and a logging sidecar. How would you ensure they share files without persistent storage?

**Solution**:  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: shared-volume-pod
spec:
  containers:
  - name: app-container
    image: nginx
    volumeMounts:
    - name: shared-vol
      mountPath: /app-logs
  - name: sidecar
    image: busybox
    command: ["sh", "-c", "tail -f /logs/access.log"]
    volumeMounts:
    - name: shared-vol
      mountPath: /logs
  volumes:
  - name: shared-vol
    emptyDir: {}
```
### 2. Pod Scheduling on GPU Nodes <a name="2-pod-scheduling-on-gpu-nodes"></a>
**Scenario:**;

Ensure ML workload Pods only schedule on GPU-equipped nodes.

Implementation:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: gpu-pod
spec:
  containers:
  - name: ml-container
    image: tensorflow-gpu
    resources:
      limits:
        nvidia.com/gpu: 2
  nodeSelector:
    accelerator: gpu
```
