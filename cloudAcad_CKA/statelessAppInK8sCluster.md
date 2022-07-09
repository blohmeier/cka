ssh ubuntu@52.42.5.7 -oStrictHostKeyChecking=no
watch -n 3 kubectl get no
kubectl describe nodes -l node-role.kubernetes.io/control-plane | more # describe the control plane node
#2 ways to "get":
k get ns #tab twice when autocomplete enabled i.e. source <(kubectl completion bash)
k get po -n kube-system # shows that All of the components of the cluster are running in pods in the kube-system namespace
