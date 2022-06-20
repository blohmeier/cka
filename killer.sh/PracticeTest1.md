<details><summary>Exam links</summary>
https://killer.sh/dashboard

</p></details>
  
### Q1 | Contexts | 1% ###
<details><summary>
<p> You have access to multiple clusters from your main terminal through kubectl contexts. Write all those context names into /opt/course/1/contexts. </p>
<p> Next write a command to display the current context into /opt/course/1/context_default_kubectl.sh, the command should use kubectl. </p>
<p> Finally write a second command doing the same thing into /opt/course/1/context_default_no_kubectl.sh, but without the use of kubectl. </p>
</summary>
<p>
  
```bash
k config get-contexts -o name > /opt/course/1/contexts
echo -e 'kubectl config current-context' > /opt/course/1/context_default_kubectl.sh
echo -e 'cat ~/.kube/config | grep current | sed -e "s/current-context: //"' > /opt/course/1/context_default_no_kubectl.sh
```
</p>
</details>

### Q2 | Schedule Pod on Master Node | 3% ###
<details><summary>
<p> Use context: kubectl config use-context k8s-c1-H </p>
<p> Create a single Pod of image httpd:2.4.41-alpine in Namespace default. The Pod should be named pod1 and the container should be named pod1-container. This Pod should only be scheduled on a master node, do not add new labels any nodes. </p>
<p> Shortly write the reason on why Pods are by default not scheduled on master nodes into the /opt/course/2/master_schedule_reason file. </p>
</summary>
<p>
  
```bash
k get node # find master node
k describe node cluster1-master1 | grep Taint # get master node taints
k describe node cluster1-master1 | grep Labels -A 10 # get master node labels
k get node cluster1-master1 --show-labels # OR: get master node labels

k run pod1 --image=httpd:2.4.41-alpine $dy > 2.yml
vim 2.yml
# edit under spec.containers:
name: pod1-container
# add spec.tolerations: and add under it:
- effect: NoSchedule                   
  key: node-role.kubernetes.io/master
# add spec.nodeSelector: and add under it:
node-role.kubernetes.io/master: ""

echo -e 'master nodes usually have a taint defined' > /opt/course/2/master_schedule_reason
```
</p>
</details>

### Q3 | Scale down StatefulSet | 1% ###
<details><summary>
<p> Use context: kubectl config use-context k8s-c1-H </p>
<p> There are two Pods named o3db-* in Namespace project-c13. C13 management asked you to scale the Pods down to one replica to save resources. Record the action. </p>
</summary>
<p>
  
```bash
k -n project-c13 get pod --show-labels | grep o3db #confirm have to work with a stateful set
k -n project-c13 scale sts o3db --replicas 1 --record

```
</p>
</details>

### Q4 | Pod Ready if Service is reachable | 4% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c1-H</p>
<p>Do the following in Namespace default. Create a single Pod named ready-if-service-ready of image nginx:1.16.1-alpine. Configure a LivenessProbe which simply runs true. Also configure a ReadinessProbe which does check if the url http://service-am-i-ready:80 is reachable, you can use wget -T2 -O- http://service-am-i-ready:80 for this. Start the Pod and confirm it isn't ready because of the ReadinessProbe.</p>
<p>Create a second Pod named am-i-ready of image nginx:1.16.1-alpine with label id: cross-server-ready. The already existing Service service-am-i-ready should now have that second Pod as endpoint.</p>
<p>Now the first Pod should be in ready state, confirm that.</p>
</summary>
<p>
  
```bash
k run ready-if-service-ready --image=nginx:1.16.1-alpine $dy > 4_pod1.yml
vim 4_pod1.yml #add under spec.containers:
livenessProbe:                               
      exec:
        command:
        - 'true'
    readinessProbe:
      exec:
        command:
        - sh
        - -c
        - 'wget -T2 -O- http://service-am-i-ready:80'
k create -f 4_pod1.yml
k run am-i-ready --image=nginx:1.16.1-alpine --labels="id=cross-server-ready"
```
</p>
</details>

### Q5 | 1% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c1-H</p>
<p>There are various Pods in all namespaces. Write a command into /opt/course/5/find_pods.sh which lists all Pods sorted by their AGE (metadata.creationTimestamp).</p>
<p>Write a second command into /opt/course/5/find_pods_uid.sh which lists all Pods sorted by field metadata.uid. Use kubectl sorting for both commands.</p>
</summary>
<p>
  
```bash
echo -e 'kubectl get pod -A --sort-by=.metadata.creationTimestamp' > /opt/course/5/find_pods.sh
echo -e 'kubectl get pod -A --sort-by=.metadata.uid' > /opt/course/5/find_pods_uid.sh
```
</p>
</details>

### Q6 | 8% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c1-H</p>
<p>Create a new PersistentVolume named safari-pv. It should have a capacity of 2Gi, accessMode ReadWriteOnce, hostPath /Volumes/Data and no storageClassName defined.</p>
<p>Next create a new PersistentVolumeClaim in Namespace project-tiger named safari-pvc . It should request 2Gi storage, accessMode ReadWriteOnce and should not define a storageClassName. The PVC should bound to the PV correctly.</p>
<p>Finally create a new Deployment safari in Namespace project-tiger which mounts that volume at /tmp/safari-data. The Pods of that Deployment should be of image httpd:2.4.41-alpine.</p>
</summary>
<p>
  
```bash

```
</p>
</details>

### Q7 | 1% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c1-H</p>
<p>The metrics-server hasn't been installed yet in the cluster, but it's something that should be done soon. Your college would already like to know the kubectl commands to:</p>
<p>1. show node resource usage</p>
<p>2. show Pod and their containers resource usage</p>
<p>Please write the commands into /opt/course/7/node.sh and /opt/course/7/pod.sh.</p>
</summary>
<p>
  
```bash

```
</p>
</details>

### Q8 | 2% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c1-H</p>
<p>Ssh into the master node with ssh cluster1-master1. Check how the master components kubelet, kube-apiserver, kube-scheduler, kube-controller-manager and etcd are started/installed on the master node. Also find out the name of the DNS application and how it's started/installed on the master node.</p>
<p>Write your findings into file /opt/course/8/master-components.txt. The file should be structured like:</p>

```
# /opt/course/8/master-components.txt
kubelet: [TYPE]
kube-apiserver: [TYPE]
kube-scheduler: [TYPE]
kube-controller-manager: [TYPE]
etcd: [TYPE]
dns: [TYPE] [NAME]
```

<p>Choices of [TYPE] are: not-installed, process, static-pod, pod</p>
</summary>
<p>
  
```bash

```
</p>
</details>

### Q9 | 5% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c2-AC</p>
<p>Ssh into the master node with ssh cluster2-master1. Temporarily stop the kube-scheduler, this means in a way that you can start it again afterwards.</p>
<p>Create a single Pod named manual-schedule of image httpd:2.4-alpine, confirm its created but not scheduled on any node.</p>
<p>Now you're the scheduler and have all its power, manually schedule that Pod on node cluster2-master1. Make sure it's running.</p>
<p>Start the kube-scheduler again and confirm its running correctly by creating a second Pod named manual-schedule2 of image httpd:2.4-alpine and check if it's running on cluster2-worker1.</p>
</summary>
<p>

```bash

```
</p>
</details>

### Q10 | 6% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c1-H</p>
<p>Create a new ServiceAccount processor in Namespace project-hamster. Create a Role and RoleBinding, both named processor as well. These should allow the new SA to only create Secrets and ConfigMaps in that Namespace.</p>
</summary>
<p>
  
```bash

```
</p>
</details>

### Q11 | 4% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c1-H</p>
<p>Use Namespace project-tiger for the following. Create a DaemonSet named ds-important with image httpd:2.4-alpine and labels id=ds-important and uuid=18426a0b-5f59-4e10-923f-c0e078e82462. The Pods it creates should request 10 millicore cpu and 10 mebibyte memory. The Pods of that DaemonSet should run on all nodes, master and worker.</p>
</summary>
<p>
  
```bash

```
</p>
</details>

### Q12 | 6% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c1-H</p>
<p>Use Namespace project-tiger for the following. Create a Deployment named deploy-important with label id=very-important (the Pods should also have this label) and 3 replicas. It should contain two containers, the first named container1 with image nginx:1.17.6-alpine and the second one named container2 with image kubernetes/pause.</p>
<p>There should be only ever one Pod of that Deployment running on one worker node. We have two worker nodes: cluster1-worker1 and cluster1-worker2. Because the Deployment has three replicas the result should be that on both nodes one Pod is running. The third Pod won't be scheduled, unless a new worker node will be added.</p>
<p>In a way we kind of simulate the behaviour of a DaemonSet here, but using a Deployment and a fixed number of replicas.</p>
</summary>
<p>
  
```bash

```
</p>
</details>

### Q13 | 4% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c1-H</p>
<p>Create a Pod named multi-container-playground in Namespace default with three containers, named c1, c2 and c3. There should be a volume attached to that Pod and mounted into every container, but the volume shouldn't be persisted or shared with other Pods.</p>
<p>Container c1 should be of image nginx:1.17.6-alpine and have the name of the node where its Pod is running available as environment variable MY_NODE_NAME.</p>
<p>Container c2 should be of image busybox:1.31.1 and write the output of the date command every second in the shared volume into file date.log. You can use while true; do date >> /your/vol/path/date.log; sleep 1; done for this.</p>
<p>Container c3 should be of image busybox:1.31.1 and constantly send the content of file date.log from the shared volume to stdout. You can use tail -f /your/vol/path/date.log for this.</p>
<p>Check the logs of container c3 to confirm correct setup.</p>
</summary>
<p>
  
```bash

```
</p>
</details>

### Q14 | 2% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c1-H</p>
<p>You're ask to find out following information about the cluster k8s-c1-H:</p>
<p>1. How many master nodes are available?</p>
<p>2. How many worker nodes are available?</p>
<p>3. What is the Service CIDR?</p>
<p>4. Which Networking (or CNI Plugin) is configured and where is its config file?</p>
<p>5. Which suffix will static pods have that run on cluster1-worker1?</p>
<p>Write your answers into file /opt/course/14/cluster-info, structured like this:</p>

```
# /opt/course/14/cluster-info
1: [ANSWER]
2: [ANSWER]
3: [ANSWER]
4: [ANSWER]
5: [ANSWER]
```
</summary>
<p>
  
```bash

```
</p>
</details>

### Q15 | 3% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c2-AC</p>
<p>Write a command into /opt/course/15/cluster_events.sh which shows the latest events in the whole cluster, ordered by time. Use kubectl for it.</p>
<p>Now kill the kube-proxy Pod running on node cluster2-worker1 and write the events this caused into /opt/course/15/pod_kill.log.</p>
<p>Finally kill the containerd container of the kube-proxy Pod on node cluster2-worker1 and write the events into /opt/course/15/container_kill.log.</p>
<p>Do you notice differences in the events both actions caused?</p>
</summary>
<p>
  
```bash

```
</p>
</details>

### Q16 | 2% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c1-H</p>
<p>Create a new Namespace called cka-master.</p>
<p>Write the names of all namespaced Kubernetes resources (like Pod, Secret, ConfigMap...) into /opt/course/16/resources.txt.</p>
<p>Find the project-* Namespace with the highest number of Roles defined in it and write its name and amount of Roles into /opt/course/16/crowded-namespace.txt.</p>
</summary>
<p>
  
```bash

```
</p>
</details>


### Q17 | 3% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c1-H</p>
<p>In Namespace project-tiger create a Pod named tigers-reunite of image httpd:2.4.41-alpine with labels pod=container and container=pod. Find out on which node the Pod is scheduled. Ssh into that node and find the containerd container belonging to that Pod.</p>
<p>Using command crictl:</p>
<p>1. Write the ID of the container and the info.runtimeType into /opt/course/17/pod-container.txt</p>
<p>2. Write the logs of the container into /opt/course/17/pod-container.log</p>
</summary>
<p>

```bash

```
</p>
</details>

### Q18 | 8% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c3-CCC</p>
<p>There seems to be an issue with the kubelet not running on cluster3-worker1. Fix it and confirm that cluster has node cluster3-worker1 available in Ready state afterwards. You should be able to schedule a Pod on cluster3-worker1 afterwards.</p>
<p>Write the reason of the issue into /opt/course/18/reason.txt.</p>
</summary>
<p>
  
```bash

```
</p>
</details>

### Q19 | 3% ###
<details><summary>
**this task can only be solved if questions 18 or 20 have been successfully implemented and the k8s-c3-CCC cluster has a functioning worker node**
<p>Use context: kubectl config use-context k8s-c3-CCC</p>
<p>Do the following in a new Namespace secret. Create a Pod named secret-pod of image busybox:1.31.1 which should keep running for some time, it should be able to run on master nodes as well.</p>
<p>There is an existing Secret located at /opt/course/19/secret1.yaml, create it in the secret Namespace and mount it readonly into the Pod at /tmp/secret1.</p>
<p>Create a new Secret in Namespace secret called secret2 which should contain user=user1 and pass=1234. These entries should be available inside the Pod's container as environment variables APP_USER and APP_PASS.</p>
<p>Confirm everything is working.</p>
</summary>
<p>

```bash

```
</p>
</details>

### Q20 | 10% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c3-CCC</p>
<p>Your coworker said node cluster3-worker2 is running an older Kubernetes version and is not even part of the cluster. Update Kubernetes on that node to the exact version that's running on cluster3-master1. Then add this node to the cluster. Use kubeadm for this.</p>
</summary>
<p>
  
```bash

```
</p>
</details>

### Q21 | 2% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c3-CCC</p>
<p>Create a Static Pod named my-static-pod in Namespace default on cluster3-master1. It should be of image nginx:1.16-alpine and have resource requests for 10m CPU and 20Mi memory.</p>
<p>Then create a NodePort Service named static-pod-service which exposes that static Pod on port 80 and check if it has Endpoints and if its reachable through the cluster3-master1 internal IP address. You can connect to the internal node IPs from your main terminal.</p>
</summary>
<p>
  
```bash

```
</p>
</details>

### Q22 | 2% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c2-AC</p>
<p>Check how long the kube-apiserver server certificate is valid on cluster2-master1. Do this with openssl or cfssl. Write the exipiration date into /opt/course/22/expiration.</p>
<p>Also run the correct kubeadm command to list the expiration dates and confirm both methods show the same date.</p>
<p>Write the correct kubeadm command that would renew the apiserver server certificate into /opt/course/22/kubeadm-renew-certs.sh.</p>
</summary>
<p>
  
```bash

```
</p>
</details>

### Q23 | 2% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c2-AC</p>
<p>Node cluster2-worker1 has been added to the cluster using kubeadm and TLS bootstrapping.</p>
<p>Find the "Issuer" and "Extended Key Usage" values of the cluster2-worker1:</p>
<p>kubelet client certificate, the one used for outgoing connections to the kube-apiserver.</p>
<p>kubelet server certificate, the one used for incoming connections from the kube-apiserver.</p>
<p>Write the information into file /opt/course/23/certificate-info.txt.</p>
<p>Compare the "Issuer" and "Extended Key Usage" fields of both certificates and make sense of these.</p>
</summary>
<p>
  
```bash

```
</p>
</details>

### Q24 | 9% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c1-H</p>
<p>There was a security incident where an intruder was able to access the whole cluster from a single hacked backend Pod.</p>
<p>To prevent this create a NetworkPolicy called np-backend in Namespace project-snake. It should allow the backend-* Pods only to:</p>
<p>- connect to db1-* Pods on port 1111</p>
<p>- connect to db2-* Pods on port 2222</p>
<p>Use the app label of Pods in your policy.</p>
<p>After implementation, connections from backend-* Pods to vault-* Pods on port 3333 should for example no longer work.</p>
</summary>
<p>
  
```bash

```
</p>
</details>

### Q25 | 8% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c3-CCC</p>
<p>Make a backup of etcd running on cluster3-master1 and save it on the master node at /tmp/etcd-backup.db.</p>
<p>Then create a Pod of your kind in the cluster./p>
<p>Finally restore the backup, confirm the cluster is still working and that the created Pod is no longer with us.</p>
</summary>
<p>
  
```bash

```
</p>
</details>
