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

```
