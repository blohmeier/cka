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
#JSONPATH options in kubectl cheat sheet AND get path to use for InternalIP with:
k get no -o json | jq -c 'paths' | grep -i InternalIP
#above yields:
["items",0,"metadata","managedFields",0,"fieldsV1","f:status","f:addresses","k:{\"type\":\"InternalIP\"}"]
["items",0,"metadata","managedFields",0,"fieldsV1","f:status","f:addresses","k:{\"type\":\"InternalIP\"}","."]
["items",0,"metadata","managedFields",0,"fieldsV1","f:status","f:addresses","k:{\"type\":\"InternalIP\"}","f:address"]
["items",0,"metadata","managedFields",0,"fieldsV1","f:status","f:addresses","k:{\"type\":\"InternalIP\"}","f:type"]
["items",1,"metadata","managedFields",1,"fieldsV1","f:status","f:addresses","k:{\"type\":\"InternalIP\"}"]
["items",1,"metadata","managedFields",1,"fieldsV1","f:status","f:addresses","k:{\"type\":\"InternalIP\"}","."]
["items",1,"metadata","managedFields",1,"fieldsV1","f:status","f:addresses","k:{\"type\":\"InternalIP\"}","f:address"]
["items",1,"metadata","managedFields",1,"fieldsV1","f:status","f:addresses","k:{\"type\":\"InternalIP\"}","f:type"]
k get no -o json | jq -c 'paths' | grep InternalIP -B 5 -A 5 #yields JSON data structure
k get no -o json | jq -c 'paths' | grep type | grep -v "metadata" | grep address
#above yields:
["items",0,"status","addresses",0,"type"]
["items",0,"status","addresses",1,"type"]
["items",1,"status","addresses",0,"type"]
["items",1,"status","addresses",1,"type"]
#i.e., can see which fields are arrays (with 0 or 1 after them) ... then, are able to provide correct jsonpath command:
k get no -o jsonpath='{.items[0].status.addresses[*].address}' #yields address for JUST FIRST NODE (.items[0]) i.e.:
10.99.4.9 controlplane
#Since only asked for internalIP, must filter out controlplane:
root@controlplane ~ ➜  k get no -o jsonpath='{.items[0].status.addresses}'
[{"address":"10.99.4.9","type":"InternalIP"},{"address":"controlplane","type":"Hostname"}]
root@controlplane ~ ➜  k get no -o jsonpath='{.items[0].status.addresses}' | jq
[
  {
    "address": "10.99.4.9",
    "type": "InternalIP"
  },
  {
    "address": "controlplane",
    "type": "Hostname"
  }
]
#To filter out, reference 'Cheat Sheet' for filter:
k get no -o jsonpath='{.items[*].status.addresses[?(@.type=="InternalIP")].address}' > /root/CKA/node_ips
```

```
3(Q):
Create a pod called multi-pod with two containers. 
Container 1, name: alpha, image: nginx
Container 2: name: beta, image: busybox, command: sleep 4800 
Environment Variables:
container 1:
name: alpha
Container 2:
name: beta
Pod Name: multi-pod
Container 1: alpha
Container 2: beta
Container beta commands set correctly?
Container 1 Environment Value Set
Container 2 Environment Value Set
```
```
3(A):
k run multi-pod --image=busybox --command $dy -- sleep 4800 > 3.yml
vim 3.yml #add name 'beta'; add name 'alpha' and details:
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: multi-pod
  name: multi-pod
spec:
  containers:
  - image: nginx
    name: alpha
    env:
      - name: "name"
        value: alpha
  - command:
    - sleep
    - "4800"
    image: busybox
    name: beta
    env:
      - name: "name"
        value: beta
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
```
4(Q): 
Create a Pod called non-root-pod , image: redis:alpine
runAsUser: 1000
fsGroup: 2000
Pod non-root-pod fsGroup configured
Pod non-root-pod runAsUser configured
```
```
4(A): 

```
