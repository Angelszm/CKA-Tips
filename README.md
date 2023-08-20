# CKA-Tips

## Kodekloud Practice and Killer sh Practice Sessions 

## Firstly need to setup vim and bash settings on playground. 

## Mind Mapping 
- https://miro.com/app/board/uXjVM0ChERI=/?share_link_id=419864773272

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

# Export to save time
```
export do="--dry-run=client -o yaml"
export now="--force --grace-period 0"
```

# Log locations to check:
```
/var/log/pods
/var/log/containers
crictl ps + crictl logs
docker ps + docker logs (in case when Docker is used)
kubelet logs: /var/log/syslog or journalctl
```

## Config File Locations to check 
```
Kubelet Config files
/var/lib/kubelet
```

Weave Network Plugin Location
```
/etc/cni/net.d/--weave--
```


## Cluster Version Upgrade with kubeadm 

Ref: https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/
Drain the node frist
```
k drain node --ignore-daemonsets
```
Upgrade steps
```
apt-mark kubeadm kubelet kubectl 
apt-get update 
apt-get install -y kubelet=1.24.0-00 kubeadm=1.24.0-00 kubectl=1.24.0-00
apt-mark hold kubelet kubectl kubeadm
kubeadm upgrade apply v1.24.0
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```

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
k explain pod.spec //View the pod spec definition
k explain pod.spec.containers //View the containers definition
k explain pod.spec.containers.resources //View the container resources definition
k explain pod.spec.containers.resources.limits //View the containter resources limits definition
k explain pod --recursive | less -S
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


Drain a node (Unschedule)
```
k drain <node-name> --ignore-daemonsets=true
k drain  <node-name> --ignore-daemonsets
```


Stop scheduling new pods onto the node:
```
k cordon node-name
```

Resume scheduling on the node
```
k uncordon node-name
```

Namespaces 
```
k create ns 
k get ns
```

Apply yaml files and folders
```
k apply -f file-name.yaml
k apply -f file-name.json
k apply -R -f . # If files are in current working directory
k apply -R -f /path/to/yaml/files
```

Expose Pod or Deployment on Services 
```
k expose -h
k expose deploy nginx --port=80 --type=ClusterIP
k expose deployment nginx --port=80 --type=NodePort
k expose deployment nginx --port=80 --type=NodePort (if we have to expose nodeport and then add nodePort field in yaml file)
k expose pod redis --port=6379 --name=redis
k expose pod redis  --type=ClusterIP --port=6379 --target-port=6379
k -n namespace expose pod pod --name service --port 3000 --target-port 80
```

Scale Replicas
```
k scale --replicas=4 deploy nginx
k edit deploy nginx (#modify replicas number) 
```

Update Deployment with record flag & Rollback
```
k set image deployment web nginx=nginx:1.14.2 --record
k rollout undo deployment web
k rollout undo deployment/web --to-revision=3
```

Service Accounts 
```
k create sa demo
```


## 4 different RBAC combinations and 3 valid ones:
Role + RoleBinding (available in single Namespace, applied in single Namespace)
ClusterRole + ClusterRoleBinding (available cluster-wide, applied cluster-wide)
ClusterRole + RoleBinding (available cluster-wide, applied in single Namespace)
Role + ClusterRoleBinding (NOT POSSIBLE: available in single Namespace, applied cluster-wide)

Roles, Rolebinding, ClusterRole and Clusterrolebinding
```
k create role demo --verb=get,list,watch --resource=pods,deploy,ds,rs,sts
k create clusterrole demo --verb='*' --resource=pods,deploy,ds,rs,sts
k create clusterrole demo --verb=get,list,watch --resource=pods,deploy,ds,rs,sts
k create rolebinding demo --role=demo --user=demo
k create rolebinding demo --clusterrole==demo --user=demo
k create clusterrolebinding demo-admin --clusterrole=demo --user=demo
```

Authentication Check: 
```
k auth can-i create deployment --as demo
```


Logging 
```
k logs pod-name
k logs deploy/name
k logs pod-name | grep 'error'
k logs pod-name --previous
k logs etcd-master
```


Metrics Server: 
```
k top pod 
k top pod --sort-by='cpu'
k top pod -l 'app=nginx' --sort-by='cpu'
```

Labels
```
k label nodes <node-name> <label-key>=<label-value>
k get all --show-labels

```


Copy files to and from pods
```
k cp --help
k cp mypod1:/dump_ops_in_flight.txt dump_ops_in_flight.txt
```


DNS Debugging 
```
k get pods dnsutils
k exec -i -t dnsutils -- nslookup  
k exec -i -t dnsutils -- sh
k exec -i -t dnsutils -- cat /etc/resolv.conf
k get endpoints kube-dns --namespace=kube-system
```


## Debugging Pods, Containers, Nodes with Crictl command
<img width="1388" alt="image" src="https://github.com/Angelszm/CKA-Tips/assets/30867160/b98b728b-cea0-4194-ac4b-73c1f416f5a9">

```
crictl ps
crictl images
crictl container_id logs
crictl inspect container_id
crictl exec -i -t container_id ls
crictl stop container_id
crictl rm container_id
crictl logs container_id
```

Custom-Columns
```
kubectl get test -o=custom-columns=NAME:.metadata.name,HANDLER:.spec.runtimeHandler,TIMESTAMP:.metadata.creationTimestamp
```

To test RBAC Setup
```
k auth can-i get secret --as system:serviceaccount:project-ns:sa-name
k auth can-i -h
```


## ETCD_BACKUP
- ETCD Member List
```
ETCDCTL_API=3 etcdctl \
 --endpoints=https://127.0.0.1:2379 \
 --cacert=/etc/etcd/pki/ca.pem \
 --cert=/etc/etcd/pki/etcd.pem \
 --key=/etc/etcd/pki/etcd-key.pem \
  member list
```

ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 --cacert=/etc/kubernetes/pki/etcd/ca.crt \
--cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key snapshot save /opt/cluser.db


ETCDCTL_API=3 etcdctl --endpoints=https://10.1.220.8:2379 --cacert=/etc/kubernetes/pki/etcd/ca.crt \
     --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key \
   snapshot save /opt/cluster1.db
   
###Be Fast 
```
k delete pod x --grace-period 0 --force
k api-resources
```

##Cert
```
openssl x509  -noout -text -in /etc/kubernetes/pki/etcd/server.crt
kubeadm certs check-expiration | grep apiserver
``` 
   
CNI File Path
   ```
   /opt/cni/bin
   /etc/cni/net.d/10-flannel.conflist
   ```




Other K8s Collections
- https://www.circonus.com/2020/12/12-critical-kubernetes-health-conditions-you-need-to-monitor-and-why/
- https://medium.com/stakater/k8s-deployments-vs-statefulsets-vs-daemonsets-60582f0c62d4
- https://sookocheff.com/post/kubernetes/understanding-kubernetes-networking-model/
- https://devopscube.com/kubernetes-ingress-tutorial/
- https://awstip.com/why-i-stumbled-upon-confluence-between-aws-load-balancers-and-kubernetes-service-c8603537ac9b
- https://kubernetes.io/docs/tutorials/stateless-application/expose-external-ip-address/
- https://www.cybereason.com/blog/how-to-prevent-out-of-memory-in-java-based-kubernetes-pods
- https://medium.com/@vipinvkmenon/running-kubectl-commands-within-a-kubernetes-job-439ed4852f68
- https://abvijaykumar.medium.com/chapter-9-observability-metrics-prometheus-grafana-alert-manager-slack-notifications-b5dcac31b462
-
- https://blog.ovhcloud.com/getting-external-traffic-into-kubernetes-clusterip-nodeport-loadbalancer-and-ingress/#:~:text=To%20reach%20the%20ClusterIp%20from%20an%20external%20computer%2C%20you%20can,(ClusterIp)%20for%20that%20Service%20
- https://neo4j.com/developer-blog/deploying-neo4j-on-kubernetes-with-helm-charts/
- https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.4/guide/integrations/external_dns/
- https://iximiuz.com/
- https://learnk8s.io/kubernetes-network-packets






----------------------------------------------------
Optional
## Other k8s useful commands
Namespace 
```
kubectl delete ns <namespace> --grace-period=0 --force
kubectl delete ns [name]
kubectl get ns [name] -o json > tmp.json

#Remove “kubernetes” from tmp.json
- kubectl proxy
test on new tab
- curl -k -H “Content-Type: application/json” -X PUT --data-binary @tmp.json 
- http://127.0.0.1:8001/api/v1/namespaces/[ns'sname]/finalize
- kubectl get ns [name]
```



### Kubernetes API-Resources
```
- https://miro.com/app/board/uXjVM0ChERI=/?share_link_id=419864773272
```
API Resources with Namespace (true/false)
- ![image](https://github.com/Angelszm/CKA-Tips/assets/30867160/788b857f-a80c-4d25-ae3e-63d198c69957)


### Kubernetes with Kind Clusters, demoapp, istio bookinof, sock-shop etc 
- ![k8s-s1-Book Demo drawio](https://github.com/Angelszm/CKA-Tips/assets/30867160/d5dc7fc0-93ec-441e-9c5f-27deee00cf69)
- ![k8s-s1-Google Cloud Microservice Demo drawio](https://github.com/Angelszm/CKA-Tips/assets/30867160/7d439f2e-444d-4021-a194-c4d45d97ea44)


### Kubernetes CRDs with ArgoCD, Istio-system, Certmanager and metallb

- Metallb APIs and CRDs 
 ![image (3)](https://github.com/Angelszm/CKA-Tips/assets/30867160/afbeac7f-d91d-4ed7-a08e-2c9423bfb276)

- Istio-System APIs and CRDs (Bookinfo)
 ![image (4)](https://github.com/Angelszm/CKA-Tips/assets/30867160/4d9398f7-18a3-4700-adf2-02d740854b65)

- Cert-Manager APIs and CRDs
   ![image (5)](https://github.com/Angelszm/CKA-Tips/assets/30867160/cf5b6bf9-964b-426c-bbe8-86ba8119651a)

- Argocd APIs and CRDs
  ![image (6)](https://github.com/Angelszm/CKA-Tips/assets/30867160/c6be5dcd-7555-4022-8927-c62570d986a2)





### Other Helm knowledge

- 3 steps to decode helm secrets
```
- base64 decode - Kubernetes secrets encoding
- base64 decode (again) - Helm encoding
- gzip decompress - Helm zipping


- kubectl get secrets sh.helm.release.v1.microservice.v1 -o json | jq .data.release | tr -d '"' | base64 -d | base64 -d | gzip -d | jq
- kubectl get secrets sh.helm.release.v1.microservice.v1 -o jsonpath='{.data.release}' | base64 -d | base64 -d | gzip -d | jq
- kubectl get secrets sh.helm.release.v1.microservice.v1 -o go-template='{{.data.release | base64decode | base64decode}}' | gzip -d | jq
```

## Title: HELM Commands
Description: The command returns the values set in the chart's values.yaml file, which specifies the configuration settings for the chart.
```
helm search repo rancher --versions
helm repo list
helm repo
helm repo add
helm search repo
helm search repo rancher -l
helm ls -A
helm create
helm install
helm list -A
helm get manifest
helm get manifest microservice
helm get manifest [RELEASE NAME] --revision
helm install --debug --dry-run microservice-helm ./yourchartname
helm install microservice ./currency-app -f values-usd.yaml --dry-run
helm pull stable/mysql
helm pull <chart-name>
helm debug my-release -n my-namespace
helm install <release-name> <package-path>
helm delete my-release -n my-namespace
helm upgrade my-release ./my-chart -n my-namespace
```


## Title: HELM Values
Description: The command returns the values set in the chart's values.yaml file, which specifies the configuration settings for the chart.
```
helm get values [RELEASE NAME] --revision
helm get values [RELEASE NAME]
helm get values ed
helm get values rancher -n cattle-system
helm get values rancher -n cattle-system
```


## Title: HELM Rollback & History 
Description: The following commands can be used for rolling back Helm releases.
```
helm rollback rancher 1 -n cattle-system {{Roll Back Revisison 1}}
helm rollback ingress-nginx 1 -n ingress-nginx
helm rollback <RELEASE> [REVISION]
helm history currency-usd-microservice
```

