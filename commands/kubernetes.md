<div align="center">

# ☸️ Kubernetes Commands

</div>

[← Back to Home](../README.md)

---

## Table of Contents

- [Cluster Information](#-cluster-information)
- [Namespaces](#-namespaces)
- [Pods](#-pods)
- [Deployments](#-deployments)
- [Services & Networking](#-services--networking)
- [ConfigMaps & Secrets](#-configmaps--secrets)
- [Volumes & Storage](#-volumes--storage)
- [Scaling & Rollouts](#-scaling--rollouts)
- [Logs & Debugging](#-logs--debugging)
- [Context & Config](#-context--config)
- [RBAC](#-rbac)

---

## 🔍 Cluster Information

```bash
kubectl version                          # Client and server version
kubectl cluster-info                     # Cluster API endpoints
kubectl get nodes                        # List cluster nodes
kubectl get nodes -o wide                # Nodes with IP, OS, etc.
kubectl describe node <node-name>        # Node details
kubectl top nodes                        # CPU/memory usage per node
kubectl api-resources                    # All available resource types
```

---

## 🗂️ Namespaces

```bash
kubectl get namespaces                   # List all namespaces
kubectl get ns                           # (short alias)
kubectl create namespace my-ns           # Create namespace
kubectl delete namespace my-ns           # Delete namespace
kubectl config set-context --current --namespace=my-ns  # Set default namespace
```

Add `-n <namespace>` to most commands to target a specific namespace:

```bash
kubectl get pods -n kube-system          # Pods in kube-system
kubectl get all -n my-ns                 # All resources in namespace
kubectl get all --all-namespaces         # All resources everywhere
kubectl get all -A                       # (short form)
```

---

## 🐳 Pods

### List and inspect

```bash
kubectl get pods                         # List pods (default namespace)
kubectl get pods -o wide                 # With node and IP info
kubectl get pods --watch                 # Watch for changes live
kubectl describe pod <pod-name>          # Detailed pod info
kubectl get pod <pod-name> -o yaml       # Full YAML definition
```

---

### Create and delete

```bash
kubectl run nginx --image=nginx          # Create a pod quickly
kubectl delete pod <pod-name>            # Delete pod
kubectl delete pod <pod-name> --force --grace-period=0   # Force delete
```

---

### Execute commands in pods

```bash
kubectl exec -it <pod-name> -- bash                   # Open bash shell
kubectl exec -it <pod-name> -- sh                     # Open sh shell
kubectl exec -it <pod-name> -c <container> -- bash    # Multi-container pod
kubectl exec <pod-name> -- cat /etc/config            # Run one-off command
```

---

### Copy files

```bash
kubectl cp <pod-name>:/path/in/pod ./local-path       # Pod → local
kubectl cp ./local-file <pod-name>:/path/in/pod       # Local → pod
```

---

## 🚀 Deployments

### Manage deployments

```bash
kubectl get deployments                              # List deployments
kubectl describe deployment <name>                   # Deployment details
kubectl create deployment nginx --image=nginx        # Create deployment
kubectl delete deployment <name>                     # Delete deployment
```

---

### Apply manifests

```bash
kubectl apply -f deployment.yaml                     # Apply/update from file
kubectl apply -f ./manifests/                        # Apply all files in dir
kubectl apply -f https://example.com/manifest.yaml   # Apply from URL
kubectl delete -f deployment.yaml                    # Delete from manifest
```

---

### Edit deployments

```bash
kubectl edit deployment <name>                       # Edit in-place
kubectl set image deployment/<name> nginx=nginx:1.25 # Update image
kubectl patch deployment <name> -p '{"spec":{"replicas":3}}'  # Patch
```

---

## 🌐 Services & Networking

```bash
kubectl get services                                 # List services
kubectl get svc                                      # (short alias)
kubectl describe service <name>                      # Service details
kubectl expose deployment <name> --port=80 --type=ClusterIP   # Expose deployment
kubectl expose deployment <name> --port=80 --type=NodePort     # NodePort
kubectl expose deployment <name> --port=80 --type=LoadBalancer # LoadBalancer
kubectl delete service <name>                        # Delete service
```

---

### Port forwarding

```bash
kubectl port-forward pod/<pod-name> 8080:80          # Forward local 8080 → pod 80
kubectl port-forward service/<svc-name> 8080:80      # Forward via service
kubectl port-forward deployment/<name> 8080:80        # Forward via deployment
```

---

### Ingress

```bash
kubectl get ingress                                  # List ingress resources
kubectl describe ingress <name>                      # Ingress details
kubectl apply -f ingress.yaml                        # Apply ingress
```

---

## 🔐 ConfigMaps & Secrets

### ConfigMaps

```bash
kubectl get configmaps                               # List ConfigMaps
kubectl describe configmap <name>                    # ConfigMap details
kubectl create configmap my-config --from-file=./config.env
kubectl create configmap my-config --from-literal=key=value
kubectl delete configmap <name>                      # Delete
```

---

### Secrets

```bash
kubectl get secrets                                  # List secrets
kubectl describe secret <name>                       # Secret details (no values)
kubectl create secret generic my-secret --from-literal=password=abc123
kubectl create secret generic my-secret --from-file=./credentials
kubectl create secret docker-registry my-reg \
  --docker-server=registry.example.com \
  --docker-username=user \
  --docker-password=pass

kubectl get secret <name> -o jsonpath='{.data.password}' | base64 --decode
kubectl delete secret <name>
```

---

## 💾 Volumes & Storage

```bash
kubectl get persistentvolumes                        # List PVs
kubectl get pv                                       # (short alias)
kubectl get persistentvolumeclaims                   # List PVCs
kubectl get pvc                                      # (short alias)
kubectl describe pvc <name>                          # PVC details
kubectl delete pvc <name>                            # Delete PVC
kubectl get storageclass                             # List storage classes
```

---

## 📈 Scaling & Rollouts

### Scaling

```bash
kubectl scale deployment <name> --replicas=3         # Scale to 3 replicas
kubectl autoscale deployment <name> --min=2 --max=5 --cpu-percent=70
kubectl get hpa                                      # List HPA resources
```

---

### Rollouts

```bash
kubectl rollout status deployment/<name>             # Check rollout progress
kubectl rollout history deployment/<name>            # Rollout history
kubectl rollout undo deployment/<name>               # Rollback to previous
kubectl rollout undo deployment/<name> --to-revision=2  # Rollback to version 2
kubectl rollout restart deployment/<name>            # Rolling restart
kubectl rollout pause deployment/<name>              # Pause rollout
kubectl rollout resume deployment/<name>             # Resume rollout
```

---

## 🐛 Logs & Debugging

### Logs

```bash
kubectl logs <pod-name>                              # Pod logs
kubectl logs <pod-name> -c <container>               # Specific container
kubectl logs <pod-name> --previous                   # Logs from crashed pod
kubectl logs -f <pod-name>                           # Follow logs live
kubectl logs <pod-name> --tail=100                   # Last 100 lines
kubectl logs -l app=nginx                            # Logs by label selector
```

---

### Debug

```bash
kubectl describe pod <pod-name>                      # Events and state
kubectl get events                                   # Cluster events
kubectl get events --sort-by='.lastTimestamp'        # Sorted by time
kubectl top pods                                     # CPU/mem per pod
kubectl top pods -n my-ns                            # In specific namespace
kubectl debug -it <pod-name> --image=busybox         # Debug sidecar
kubectl run debug --image=busybox -it --rm           # Temp debug pod
```

---

## 🔧 Context & Config

```bash
kubectl config get-contexts                          # List contexts
kubectl config current-context                       # Show active context
kubectl config use-context <name>                    # Switch context
kubectl config set-context <name> --namespace=my-ns  # Set namespace for context
kubectl config view                                  # View kubeconfig
```

---

## 🔒 RBAC

```bash
kubectl get roles                                    # List roles
kubectl get rolebindings                             # List role bindings
kubectl get clusterroles                             # List cluster roles
kubectl get clusterrolebindings                      # List cluster role bindings
kubectl describe role <name>                         # Role permissions
kubectl auth can-i create pods                       # Check own permissions
kubectl auth can-i create pods --as=system:serviceaccount:default:my-sa
```

---

### Useful shortcuts

| Short | Full |
|-------|------|
| `po` | `pods` |
| `svc` | `services` |
| `deploy` | `deployments` |
| `ns` | `namespaces` |
| `cm` | `configmaps` |
| `sa` | `serviceaccounts` |
| `pv` | `persistentvolumes` |
| `pvc` | `persistentvolumeclaims` |
| `ing` | `ingress` |

```bash
kubectl get po,svc,deploy -n my-ns        # Multiple resource types at once
kubectl get po -l app=nginx               # Filter by label
kubectl get po --field-selector status.phase=Running
```

---

[← Git](git.md) | [Back to Home](../README.md) | [📦 npm/Yarn →](npm-yarn.md)
