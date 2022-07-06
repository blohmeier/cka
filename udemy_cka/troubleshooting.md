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
#Check node status - Ready or NotReady?
k get no
#if NotReady:
k describe node <name>
#if "Status"=Unknown or True (except "Ready"), check LastHeartBeatTime then check status of node itself; if crashed bring it back up and check for possible cpu, mem or disk space on the nodes:
top
df -h
#Check kubelet status & check logs for possible issues: 
service kubelet status
sudo journalctl -u kubelet
#check kubelet certs, ensure not expired, are part of right group, and that certs are issued by the correct ca:
openssl x509 -in /var/lib/kubelet/worker-1.crt -text

```
