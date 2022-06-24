```
§7-Security
k api-resources --namespaced=true #or =false
k get roles -A --no-headers | wc -l
```

```
logs for troubleshooting
kubectl logs –f pod-name container-name

/var/log/pods
/var/log/containers

crictl ps + crictl logs OR FOR DOCKER: docker ps + docker logs

#kubelet logs: 
tail -f /var/log/syslog | grep apiserver
journalctl | grep apiserver

#from 7-Security 134-145
journalctl -u etcd.service -l
```

```
§8-Networking
ip link                                     #list/modify host interfaces
ip addr                                     #see ip addresses assigned to interfaces
ip addr add 192.168.1.10/24 dev eth0        #set up addresses in ifs. 
                                            #To persist after restart, modify /etc/network/interfaces
route OR ip route                           #view IP routing table
ip route add 192.168.2.0/24 via 192.168.1.1 #add ip route table entries
cat /proc/sys/net/ipv4/ip_forward           #check if ip forwarding enabled on a host, if working on host config'd as router.
```
