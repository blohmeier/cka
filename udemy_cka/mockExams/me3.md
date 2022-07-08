1a:create sa 'pvviewer', grant it access to list all pvs in the cluster by creating ClusterRole 'pvviewer-role' and ClusterRoleBinding 'pvviewer-role-binding'.
```
k create sa pvviewer
#k create clusterrole pvviewer-role --verb=list --resource=per #"park it" to confirm pv wording
k api-resources | grep persistent
k create clusterrole pvviewer-role --verb=list --resource=persistentvolumes
k create clusterrolebinding pvviewer-role-binding --clusterrole=pvviewer-role --serviceaccount=default:pvviewer
```
1b:create pod pvviewer w/image redis and sa pvviewer in default ns.
```
k run pvviewer --image=redis --serviceaccount=pvviewer
k get pods --watch #pvviewer goes from ContainerCreating to Running OR watch on 3s interval with:
watch -n 3 kubectl get po
```
2:List the InternalIP of all nodes of the cluster. Save the result to a file /root/CKA/node_ips.
Answer should be in the format: InternalIP of controlplane<space>InternalIP of node01 (in a single line)
```
#JSONPATH options in kubectl cheat sheet AND visualize better with:
k get no -o json | jq -c 'paths' grep -i InternalIP #
k get no -o json | jq -c 'paths' grep InternalIP -B 5 -A 5
```
