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
