<details><summary>Exam links</summary>
https://killer.sh/dashboard

</p></details>
  
### Q1 | 1% ###
<details><summary>
<p> You have access to multiple clusters from your main terminal through kubectl contexts. Write all those context names into /opt/course/1/contexts. </p>
<p> Next write a command to display the current context into /opt/course/1/context_default_kubectl.sh, the command should use kubectl. </p>
<p> Finally write a second command doing the same thing into /opt/course/1/context_default_no_kubectl.sh, but without the use of kubectl. </p>
</summary>
<p>
  
```bash

```
</p>
</details>

### Q2 | 3% ###
<details><summary>
<p> Use context: kubectl config use-context k8s-c1-H </p>
<p> Create a single Pod of image httpd:2.4.41-alpine in Namespace default. The Pod should be named pod1 and the container should be named pod1-container. This Pod should only be scheduled on a master node, do not add new labels any nodes. </p>
<p> Shortly write the reason on why Pods are by default not scheduled on master nodes into the /opt/course/2/master_schedule_reason file. </p>
</summary>
<p>
  
```bash

```
</p>
</details>

### Q3 | 1% ###
<details><summary>
<p> Use context: kubectl config use-context k8s-c1-H </p>
<p> There are two Pods named o3db-* in Namespace project-c13. C13 management asked you to scale the Pods down to one replica to save resources. Record the action. </p>
</summary>
<p>
  
```bash

```
</p>
</details>

### Q4 | 4% ###
<details><summary>
<p>Use context: kubectl config use-context k8s-c1-H</p>
<p>Do the following in Namespace default. Create a single Pod named ready-if-service-ready of image nginx:1.16.1-alpine. Configure a LivenessProbe which simply runs true. Also configure a ReadinessProbe which does check if the url http://service-am-i-ready:80 is reachable, you can use wget -T2 -O- http://service-am-i-ready:80 for this. Start the Pod and confirm it isn't ready because of the ReadinessProbe.</p>
<p>Create a second Pod named am-i-ready of image nginx:1.16.1-alpine with label id: cross-server-ready. The already existing Service service-am-i-ready should now have that second Pod as endpoint.</p>
<p>Now the first Pod should be in ready state, confirm that.</p>
</summary>
<p>
  
```bash

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

### Q18 | Service misconfiguration ###
<details><summary>
There seems to be an issue in Namespace mars where the ClusterIP service manager-api-svc should make the Pods of Deployment manager-api-deployment available inside the cluster.
You can test this with curl manager-api-svc.mars:4444 from a temporary nginx:alpine Pod. Check for the misconfiguration and apply a fix.
</summary>
<p>
  
```bash
1of2:Diagnose
k -n mars get all #All pods STATUS: "Running"; gets you svc NAME:PORT and CLUSTER-IP for later
k -n mars run tmp --restart=Never --rm -i --image=nginx:alpine -- curl -m 5 manager-api-svc:4444 #can't connect to svc
k -n mars get ep #no eps
k -n mars run tmp --restart=Never --rm -i --image=nginx:alpine -- curl -m 5 <test cluster IP> #pod connects
```
</p>
</details>

<details><summary> ... apply a fix.
</summary>
<p>
  
```bash
2of2
k -n mars edit svc #spec.selector should be manager-api-pod
k -n mars run tmp --restart=Never --rm -i --image=nginx:alpine -- curl -m 5 manager-api-svc:4444 #svc works now
k -n mars get ep #eps show now
```
</p>
</details>

### Q19 | Service ClusterIP->NodePort ###
<details><summary>
1of4 In Namespace jupiter you'll find an apache Deployment (with one replica) named jupiter-crew-deploy and a ClusterIP Service called jupiter-crew-svc which exposes it. Change this service to a NodePort one to make it available on all nodes on port 30100.
</summary>
<p>

```bash
k -n jupiter run tmp --restart=Never --rm -i --image=nginx:alpine -- curl -m 5 jupiter-crew-svc:8080 #ClusterIP svc jupiter-crew-svc does work
k -n jupiter edit svc jupiter-crew-svc
nodePort: 30100 #spec.ports
type: NodePort #spec
```
</p>
</details>

<details><summary>
2of4 Test the NodePort Service using the internal IP of all available nodes and the port 30100 using curl, you can reach the internal node IPs directly from your main terminal. 
</summary>
<p>
  
```bash
#must know for ports!: https://nigelpoulton.com/explained-kubernetes-service-ports/?force_isolation=true
k -n jupiter run tmp --restart=Never --rm -i --image=nginx:alpine -- curl -m 5 jupiter-crew-svc:8080 #ClusterIP svc is still internally reachable
```
</p>
</details>

<details><summary>
3of4 On which nodes is the Service reachable? 
</summary>
<p>
  
```bash
k get nodes -o wide #get <masterIP> and <workerIP> for testing whether svc is reachable
curl <masterIP>:30100 && curl <workerIP>:30100 # html><body><h1>It works!</h1></body></html>
```
</p>
</details>

<details><summary>
4of4 On which node is the Pod running?
</summary>
<p>
  
```bash
k -n jupiter get pod jupiter-crew-deploy-<dep>-<pod> -o yaml | grep nodeName #confirm which node pod runs on
```
</p>
</details>

### Q20 | NetworkPolicy ###
<details><summary>
1of2 In Namespace venus you'll find two Deployments named api and frontend. Both Deployments are exposed inside the cluster using Services. Create a NetworkPolicy named np1 which restricts outgoing tcp connections from Deployment frontend and only allows those going to Deployment api. Make sure the NetworkPolicy still allows outgoing traffic on UDP/TCP ports 53 for DNS resolution.
</summary>
<p>
  
```bash
k -n venus exec frontend-<dep>-<pod> -- wget -T 10 -O- www.google.com #works
k -n venus exec frontend-<dep>-<pod> -- wget -T 10 -O- api:2222 #works
vim 20_netpol.yml #copy from documentation and edit as shown
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: np1
  namespace: venus
spec:
  podSelector:
    matchLabels:
      id: frontend    # label of the pods to which this policy applies
  policyTypes:
  - Egress            # only control egress
  egress:
  - to:               # rule 1 - egress only to pods with api label
    - podSelector:   
        matchLabels:
          id: api
  - ports:            # rule 2 - UDP/TCP DNS on port 53 ("-" = OR (without, these would be 1 rule)).
    - port: 53        
      protocol: UDP
    - port: 53        
      protocol: TCP
k create -f 20_netpol.yml
```
</p>
</details>

<details><summary>
2of2 Test using: wget www.google.com and wget api:2222 from a Pod of Deployment frontend.
</summary>
<p>
  
```bash
k -n venus exec frontend-<dep>-<pod> -- wget -T 10 -O- www.google.com #no longer working
k -n venus exec frontend-<dep>-<pod> -- wget -T 10 -O- api:2222 #works
```
</p>
</details>

### Q21 | Requests and Limits, ServiceAccount ###
<details><summary>
Team Neptune needs 3 Pods of image httpd:2.4-alpine, create a Deployment named neptune-10ab for this. The containers should be named neptune-pod-10ab. Each container should have a memory request of 20Mi and a memory limit of 50Mi. Team Neptune has its own ServiceAccount neptune-sa-v2 under which the Pods should run. The Deployment should be in Namespace neptune.
</summary>
<p>
  
```bash
k -n neptune create deploy neptune-10ab --image=httpd:2.4-alpine --replicas=3 $dy > 21.yaml
vim 21.yml
#spec.template.spec:
serviceAccountName: neptune-sa-v2 
#spec.template.spec.containers:
name: neptune-pod-10ab  # change
resources:              # add
  limits:               # add
    memory: 50Mi        # add
  requests:             # add
    memory: 20Mi        # add

```
</p>
</details>

### Q22 | Labels, Annotations ###
<details><summary>
Team Sunny needs to identify some of their Pods in namespace sun. They ask you to add a new label protected: true to all Pods with an existing label type: worker or type: runner. Also add an annotation protected: do not delete this pod to all Pods having the new label protected: true.
</summary>
<p>
  
```bash
k -n sun label pod -l "type in (worker,runner)" protected=true
k -n sun annotate pod -l protected=true protected="do not delete this pod"
```
</p>
</details>

### Preview Question 1 ###
<details><summary>
In Namespace pluto there is a Deployment named project-23-api. It has been working okay for a while but Team Pluto needs it to be more reliable. Implement a liveness-probe which checks the container to be reachable on port 80. Initially the probe should wait 10, periodically 15 seconds.
The original Deployment yaml is available at /opt/course/p1/project-23-api.yaml. Save your changes at /opt/course/p1/project-23-api-new.yaml and apply the changes.
</summary>
<p>
  
```bash
cp /opt/course/p1/project-23-api.yaml /opt/course/p1/project-23-api-new.yaml
vim /opt/course/p1/project-23-api-new.yaml #add below under under spec.template.spec.containers
livenessProbe:
  tcpSocket:
    port: 80
  initialDelaySeconds: 10
  periodSeconds: 15
k delete deploy -n pluto project-23-api 
k create -f /opt/course/p1/project-23-api-new.yaml
```
</p>
</details>

### Preview Question 2 ###
<details><summary>
Team Sun needs a new Deployment named sunny with 4 replicas of image nginx:1.17.3-alpine in Namespace sun. The Deployment and its Pods should use the existing ServiceAccount sa-sun-deploy.
Expose the Deployment internally using a ClusterIP Service named sun-srv on port 9999. The nginx containers should run as default on port 80. The management of Team Sun would like to execute a command to check that all Pods are running on occasion. Write that command into file /opt/course/p2/sunny_status_command.sh. The command should use kubectl.
</summary>
<p>
  
```bash
k create deploy sunny -n sun --image=nginx:1.17.3-alpine --replicas=4 $dy > pq2.ym
pq2.yml
vim pq2.yml # add "serviceAccountName: sa-sun-deploy" under spec.template.spec
k create -f pq2.yml
k expose deploy -n sun sunny --type=ClusterIP --port=80 --target-port=9999 --name=sun-srv
echo "kubectl get pods -l app=sunny -n sun" > /opt/course/p2/sunny_status_command.sh
```
</p>
</details>

### Preview Question 3 ###
<details><summary>
Management of EarthAG recorded that one of their Services stopped working. Dirk, the administrator, left already for the long weekend. All the information they could give you is that it was located in Namespace earth and that it stopped working after the latest rollout. All Services of EarthAG should be reachable from inside the cluster.
Find the Service, fix any issues and confirm its working again. Write the reason of the error into file /opt/course/p3/ticket-654.txt so Dirk knows what the issue was.
</summary>
<p>
  
```bash

```
</p>
</details>
