```
#obtain pod label and accompanying service selector (should match; if not, one must change):
kubectl -n gamma describe pod mysql | grep -i label
kubectl -n gamma describe svc mysql-service | grep -i selector
```
```
k logs <podname> -f --previous #view logs of previously running pod
```
```
Controlplane Failure Troubleshooting Steps:
#Check node status
k get no; k get po
#check controlplane pods
k get po -n kube-system
#or if components deployed as services:
service kube-apiserver status
service kube-controller-manager status
service kube-scheduler status
```
