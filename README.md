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
```
alias k='kubectl'
alias kdp='kubectl describe pod'
alias ke='kubectl explain'
alias kc='kubectl create -f'
alias kgp='kubectl get pods --show-labels'
alias kr='kubectl replace -f'
alias kn='kubectl get namespaces'
```


# Display Kubernetes API Server URL
$ k cluster-info

# Dump all cluster information
$ k cluster-info dump

# Get kubeadm config
$ k -n kube-system get configmap kubeadm-config -o yaml

# List all nodes in the cluster
$ k get nodes

# Check health of cluster components [Like Controller Manager, Scheduler, ETCD Server etc]
$ k get componentstatuses
$ k get cs

# List all API resources {}
$ k api-resources
$ k api-resources --namespaced=true {Print related all namespaces resources}

# List API versions
$ k api-versions


Pods Creation 
k run test-pod --image=redis 
k run test-pod --image=redis --command -- sleep 1000
k run test-pod --image=redis --lables=pod=test,id=test
k run test-pod --image=redis --command -- 'echo' 'echo no need' 'echo bye'

