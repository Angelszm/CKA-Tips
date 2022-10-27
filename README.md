# CKA-Tips


## Firstly need to setup vim and bash settings on playground. 



For Vim 
```
set nu ##Enable Line Numbers
set expandtab ##use spaces for tab
set tapstop=2  ##amount of spaces used for tab
set shiftwidth=2 ##amount of spaces during indentation
```

For Bash 
(can update in bashrc file also)
```
echo 'source <(kubectl completion bash)' >>~/.bashrc
source ~/.bashrc
```
Aliases
```
alias k='kubectl'
alias ke='kubectl explain'
alias kcf='kubectl create -f'
alias kaf='kubectl apply -f'
alias kgp='kubectl get pods'
alias kgpa='kubectl get pods --all-namespaces'
alias kgd='kubectl get deployments'
alias kgs='kubectl get service'
alias kgh='kubectl get hpa'
alias kdp='kubectl describe pod'
alias kr='kubectl replace -f'
alias kn='kubectl get namespaces'
```

# Delete resources
alias kd='kubectl delete'
alias kdp='kubectl delete pods'
alias kdd='kubectl delete deployments'
alias kgs='kubectl delete service'


# Cluster Info and Commands
Display Kubernetes API Server URL
```
$ k cluster-info
```

Dump all cluster information
```
$ k cluster-info dump
```

Get kubeadm config
```
$ k -n kube-system get configmap kubeadm-config -o yaml
```

List all nodes in the cluster
```
$ k get nodes
```

Check health of cluster components [Like Controller Manager, Scheduler, ETCD Server etc]
```
$ k get componentstatuses
$ k get cs
```

List all API resources {}
```
$ k api-resources
$ k api-resources --namespaced=true {Print related all namespaces resources}
```

List API versions
```
$ k api-versions
```

# Cluster Contexts
Get List contexts
```
$ k config get-contexts
```

Get Current Current ( Display the current-context)
```
$ k config current-context
```

Switch between Clusters 
```
$ k config use-context 
$ k config set-context (first time setting a context)
```

Get kubeadm config
```
$ k -n kube-system get configmap kubeadm-config -o yaml
```


Pods Creation 
```
k run test-pod --image=redis 
k run test-pod --image=redis --command -- sleep 1000
k run test-pod --image=redis --lables=pod=test,id=test
k run test-pod --image=redis --command -- 'echo' 'echo no need' 'echo bye'
```


Validate Manifest File 
```
k create --dry-run --validate -f <file>.yaml
```


Drain a node 
```
k drain <node-name> --ignore-daemonsets=true
```

Apply yaml files and folders
```
k apply -f file-name.yaml
k apply -f file-name.json
k apply -R -f . # If files are in current working directory
k apply -R -f /path/to/yaml/files
```

