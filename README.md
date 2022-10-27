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
k run test-pod --image=busybox --command -- 'sh' '-c' 'wget ...'
k run test-pod --image=busybox --command -- '/bin/sleep' '1d'
k run --rm -it --image=alpine alpine -- sh (# for testing purpose and will die after exit)
k run nginx --image=nginx --restart=Never --dry-run=client -o yaml
k run nginx --image=nginx --restart=Never --limits='cpu=300m,memory=512Mi' --dry-run=client -o yaml
k run nginx --image=nginx --restart=Never --requests='cpu=100m,memory=256Mi' --limits='cpu=300m,memory=512Mi' --dry-run=client -o yaml

```

Shell Session to the Pod: (exec commands)
```
k exec --stdin --tty test-pod -- sh
k exec --stdin --tty test-pod -- /bin/bash
k exec --stdin --tty test-pod -- nslookup 
k exec --stdin --tty test-pod -- ls -l /etc/hosts
k exec --stdin --tty test-pod -- apt update
k exec --stdin --tty test-pod -- cat /etc/os-release
k exec --stdin --tty test-pod -- env
```


Explain Resources 
```
k explain pod
k explain deploy
k explain service
k explain deploy.spec
k explain deploy.spec.replicas
k explain pod.metadata.namespace
k explain deploy.spec.template.spec.tolerations
```

Deployments 
```
k create deploy nginx --image=nginx  --replicas=3 --dry-run=client -o yaml
k create deploy nginx --image=nginx  --replicas=3 --dry-run=client -o yaml >nginx-deployment.yml


```

Sort-By
```
k get svc --sort-by=.metadata.name --all-namespaces
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

Namespaces 
```
k create ns 
k get ns
```


Service Accounts 
```

```


Roles, Rolebinding, ClusterRole and Clusterrolebinding
```

```
