```
PROVISION VMS WITH VAGRANT
git clone https://github.com/kodekloudhub/certified-kubernetes-administrator-course.git
cd certified-kubernetes-administrator-course
vi Vagrantfile
vagrant status #vms in 'not created' state
vagrant up #node provisioning time depends on resources
vagrant status #all vms in running state
vagrant ssh kubemaster #ssh into master node
uptime #just make sure commands working
logout
vagrant ssh kubenode01
logout
vagrant ssh kubenode02
```

```
BOOTSTRAP CLUSTER USING KUBEADM
kubadm documentation links (updated?): 
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/?force_isolation=true

lsmod | grep br_netfilter #ensure this module is loaded before step of ensuring iptables see bridged traffic
sudo modprobe br_netfilter #load module explicitly (if no output from command above)
#repeat above for all nodes. Then, run below to create new kernel parameters:

```

```
PRACTICE - DEPLOY CLUSTER USING KUBEADM
#1) Install the kubeadm and kubelet packages on the controlplane and node01. Use the exact version of 1.23.0-00 for both.

##First, set net.bridge.bridge-nf-call-iptables to 1:
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sudo sysctl --system

##Second, install docker runtime (has already been installed on both nodes, so skip here)

##Third, Install kubeadm, kubectl and kubelet on all nodes:
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubelet=1.23.0-00 kubeadm=1.23.0-00 kubectl=1.23.0-00
sudo apt-mark hold kubelet kubeadm kubectl


```
