```
#obtain pod label and accompanying service selector (should match; if not, one must change):
kubectl -n gamma describe pod mysql | grep -i label
kubectl -n gamma describe svc mysql-service | grep -i selector
```
```
k logs <podname> -f --previous #view logs of previously running pod
```
```
Control Plane Failure Troubleshooting Steps:
#1: Check node status
k get no; k get po
#2: check controlplane pods
#if kubeadm
k get po -n kube-system
#or if components deployed as services:
service kube-apiserver status
service kube-controller-manager status
service kube-scheduler status
#3: Check Service Logs (of control plane components)
#if kubeadm, check logs of pods hosting control plane components:
k logs kube-apiserver-master -n kube-system
#or in case of svcs configured natively:
sudo journalctl -u kube-apiserver
#more tips are at:
https://kubernetes.io/docs/tasks/debug/debug-cluster/
```
```
Worker Node Failure Troubleshooting Steps:
#1: Check node status - Ready or NotReady?
k get no
#if NotReady:
k describe node <name>
#2: 
```
