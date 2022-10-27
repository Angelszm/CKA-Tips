# kubernetes-jsonpath

Mostly can check here: https://github.com/Angelszm/kubernetes-jsonpath#readme

JSONPATH: 
nodeName & Namespace
```
kubectl get pods --all-namespaces --output 'jsonpath={range .items[*]}{.spec.nodeName}{" "}{.metadata.namespace}{" "}{.metadata.name}{"\n"}{end}'
```

Status & Node Names
```
kubectl get pod -o=custom-columns=NAME:.metadata.name,STATUS:.status.phase,NODE:.spec.nodeName --all-namespaces
```

Kubelet Version 
```
kubectl get node -o custom-columns=NAME:.metadata.name,VERSION:.status.nodeInfo.kubeletVersion 
```

CPU & Memory
```
kubectl get po -o custom-columns="Name:metadata.name,Memory-limit:spec.containers[*].resources.limits.memory,Memory-Requests:spec.containers[*].resources.requests.memory,CPU-limit:spec.containers[*].resources.limits.cpu,CPU-Requests:spec.containers[*].resources.requests.cpu" -n namespace
```

Check which nodes are ready
```
JSONPATH='{range .items[*]}{@.metadata.name}:{range @.status.conditions[*]}{@.type}={@.status};{end}{end}' \
 && kubectl get nodes -o jsonpath="$JSONPATH" | grep "Ready=True"
```
 
Node IP Addresses 
```
kubectl get nodes  -o jsonpath='{.items[*].status.addresses[]}'
```








Outputs
# Get Current Currents ( Display the current-context)
```
kubectl config current-context
```

# Get Contexts from the cluster
```
kubectl config get-contexts
```