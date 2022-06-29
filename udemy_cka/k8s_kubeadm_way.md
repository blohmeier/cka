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
kubadm documentation link: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
lsmod | grep br_netfilter #ensure this module is loaded before step of ensuring iptables see bridged traffic
sudo modprobe br_netfilter #load module explicitly

```
