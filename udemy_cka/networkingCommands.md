```
service n/w practice test 

#Get IP address range of the n/w the cluster nodes are part of:
apt update && apt install ipcalc
ip a | grep eth0 #reveals ip and subnet mask of 'controlplane'/'master' node - should match op of k get nodes -o wide
ipcalc -b <ip and subnet mask from command above>

#Get IP address range of pods in cluster (configured with weave):
k get po -o wide -A #gets each <weave-pod-name>
k -n kube-system logs <weave-pod-name> weave | grep -i ipalloc

#Get IP address range of services in cluster:
cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep cluster-ip-range #inspect setting on kube-api server

#Get number of kube-proxy pods deployed in the cluster:
k -n kube-system get po | grep -i proxy

#Get type of proxy kube-proxy is configured to use:
k -n kube-system get po | grep -i proxy #reveals each kube-proxy pod name
k -n kube-system logs <kube-proxy-pod-name> #look for "proxy" in log entry. Default is ip-tables.

#Reveals how cluster ensures a kube-proxy pod runs on all nodes in the vluster:
k -n kube-system get ds
```

```
DNS in k8s
#view kubelet manifest
cat /var/lib/kubelet/config.yaml



```

```
Ingress Networking
#get from to http://my-online-store.com



```
