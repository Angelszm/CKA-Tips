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
