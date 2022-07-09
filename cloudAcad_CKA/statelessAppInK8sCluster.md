### SSH into cluster; verify all cluster components are up and running ###
```
ssh ubuntu@52.42.5.7 -oStrictHostKeyChecking=no
watch -n 3 kubectl get no
kubectl describe nodes -l node-role.kubernetes.io/control-plane | more # describe the control plane node
#2 ways to "get":
k get ns #tab twice when autocomplete enabled i.e. source <(kubectl completion bash)
k get po -n kube-system # shows that All of the components of the cluster are running in pods in the kube-system namespace i.e.:
calico: The container network used to connect each node to every other node in the cluster. Calico also supports network policy. Calico is one of many possible container networks that can be used by Kubernetes.
coredns: Provides DNS services to nodes in the cluster
etcd: The primary data store of all cluster state
kube-apiserver: The REST API server for managing the Kubernetes cluster
kube-controller-manager: Manager of all of the controllers in the cluster that monitor and change the cluster state when necessary
kube-proxy: Network proxy that runs on each node
kube-scheduler: Control plane process which assigns Pods to Nodes
metrics-server: Not an essential component of a Kubernetes cluster but it is used in this lab to provide metrics for viewing in the Kubernetes dashboard.
```

```
kubectl get pods -l app=mysql --watch #get pods of a certain label; write any updates to the output
```

### With MySQL up and running, execute commands against the db to demonstrate it works and k8s can gracefully handle failures and other requests. ###
```
#create a mydb database with a 'notes' table in it with one record
kubectl run mysql-client --image=mysql:5.7 -i -t --rm --restart=Never -- /usr/bin/mysql -h mysql-0.mysql -e "CREATE DATABASE mydb; CREATE TABLE mydb.notes (note VARCHAR(250)); INSERT INTO mydb.notes VALUES ('k8s Cloud Academy Lab');"

#Run a query using the mysql-read endpoint to select all of the notes in the table:
kubectl run mysql-client --image=mysql:5.7 -i -t --rm --restart=Never -- /usr/bin/mysql -h mysql-read -e "SELECT * FROM mydb.notes"

#Run an SQL command that outputs the MySQL server's ID to confirm that the requests are distributed to different pods
kubectl run mysql-client-loop --image=mysql:5.7 -i -t --rm --restart=Never -- bash -ic "while sleep 1; do /usr/bin/mysql -h mysql-read -e 'SELECT @@server_id'; done"
#list pods; include 'node name' column
k get po -o wide

#simulate taking the node running the mysql-2 pod out of service for maintenance:
node=$(kubectl get pods --field-selector metadata.name=mysql-2 -o=jsonpath='{.items[0].spec.nodeName}')
kubectl drain $node --force --delete-local-data --ignore-daemonsets # prevents new pods from being scheduled on the node; evicts existing pods scheduled to it
k get po -o wide --watch # Watch the mysql-2 pod get rescheduled to a different node
kubectl uncordon $node #Uncordon the node you drained so that pods can be scheduled on it again

kubectl delete pod mysql-2 # Delete the mysql-2 pod to simulate a node failure
kubectl get pod mysql-2 -o wide --watch # watch it get rescheduled automatically

kubectl scale --replicas=5 statefulset mysql
kubectl get pods -l app=mysql --watch

#verify you see new MySQL server IDs
kubectl run mysql-client-loop --image=mysql:5.7 -i -t --rm --restart=Never -- bash -ic "while sleep 1; do /usr/bin/mysql -h mysql-read -e 'SELECT @@server_id'; done"

#confirm data is replicated in new mysql-4 pod
kubectl run mysql-client --image=mysql:5.7 -i -t --rm --restart=Never -- /usr/bin/mysql -h mysql-4.mysql -e "SELECT * FROM mydb.notes"

kubectl get services mysql-read #Display the internal virtual IP of the mysql-read endpoint
echo "  type: LoadBalancer" >> mysql-services.yaml #Append a load balancer type to the mysql-read service declaration
```
