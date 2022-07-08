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

k create -f 3.yml
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
k run non-root-pod --image=redis:alpine $dy > 4.yml
vim 4.yml #search for securityContext for correct edits:
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: non-root-pod
  name: non-root-pod
spec:
  securityContext:
    runAsUser: 1000
    fsGroup: 2000
  containers:
  - image: redis:alpine
    name: non-root-pod
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

k create -f 4.yml

k get po non-root-po -o yaml #verify securityContext,runAsUser,fsGroup
```
```
5(Q):
We have deployed a new pod called np-test-1 and a service called np-test-service. Incoming connections to this service are not working. Troubleshoot and fix it.
Create NetworkPolicy, by the name ingress-to-nptest that allows incoming connections to the service over port 80.
Important: Don't delete any current objects deployed.
Important: Don't Alter Existing Objects!
NetworkPolicy: Applied to All sources (Incoming traffic from all pods)?
NetWorkPolicy: Correct Port?
NetWorkPolicy: Applied to correct Pod?
```
```
5(A)
#FIRST test. SECOND apply fix. THIRD test again.

#FIRST test.
Temp container (could be netcat in busybox or since just port 80, use alpine curl):
k run curl --image=alpine/curl --rm -it -- sh
curl np-test-service
#no response ... open second terminal to fix the issue

#SECOND apply fix. In second terminal, search for NetworkPolicy and edit template:
vi 5.yml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: ingress-to-nptest
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: np-test-1  # get value by running in separate terminal: k get po np-test-1 -o yaml | grep -i ' labels' -A3 ## yields: 
                      # labels:
                      #   run: np-test-1
                      # managedFields:
                      # - apiVersion: v1
  policyTypes:
    - Ingress
  ingress:
    - 
      ports:
        - protocol: TCP
          port: 80

k create -f 5.yml

#THIRD test again. Should bet nginx test page:
k run curl --image=alpine/curl --rm -it -- sh
curl np-test-service
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title...
```
```
6(Q):
Taint the worker node node01 to be Unschedulable. Once done, create a pod called dev-redis, image redis:alpine, to ensure workloads are not scheduled to this worker node. Finally, create a new pod called prod-redis and image: redis:alpine with toleration to be scheduled on node01.
key: env_type, value: production, operator: Equal and effect: NoSchedule
Key = env_type
Value = production
Effect = NoSchedule
pod 'dev-redis' (no tolerations) is not scheduled on node01?
Create a pod 'prod-redis' to run on node01
```
```
6(A)
```
k taint no node01 env_type=production:NoSchedule
k run dev-redis --image=redis:alpine
k run prod-redis --image=redis:alpine $dy > 6.yml
vi 6.yml #search for 'tolerations' to add at end:
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: prod-redis
  name: prod-redis
spec:
  containers:
  - image: redis:alpine
    name: prod-redis
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  tolerations:
  - key: "env_type"
    operator: "Equal"
    value: "production"
    effect: "NoSchedule"
status: {}
```
```
7(Q):
Taint the worker node node01 to be Unschedulable. Once done, create a pod called dev-redis, image redis:alpine, to ensure workloads are not scheduled to this worker node. Finally, create a new pod called prod-redis and image: redis:alpine with toleration to be scheduled on node01.
key: env_type, value: production, operator: Equal and effect: NoSchedule
Key = env_type
Value = production
Effect = NoSchedule
pod 'dev-redis' (no tolerations) is not scheduled on node01?
Create a pod 'prod-redis' to run on node01
```
```
7(A)
```
k create ns hr
k -n hr run hr-pod --image=redis:alpine --labels="environment=production,tier=frontend"
```
```
8(Q):
A kubeconfig file called super.kubeconfig has been created under /root/CKA. There is something wrong with the configuration. Troubleshoot and fix it.
Fix /root/CKA/super.kubeconfig
```
```
8(A)
```
#specify kubeconfig file other than default (in /.kube directory)
k get no --kubeconfig
```
