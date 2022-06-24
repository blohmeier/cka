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


cat >> /etc/hosts                           #use stdin to add entry to hosts file for ip address = hostname correlation
hostname
cat /etc/resolv.conf                        #(1)  add 'nameserver <IP address>' entry to configure DNS server address at a <p> host
                                            #(2)  commmon nameservers: 8.8.8.8 (hosted by google)
                                            #     ALTERNATIVE: add dns server entry: Forward All to 8.8.8.8
                                            #(3)  add 'search <subdomain.domain>.com' entry for alternative domain when
                                            #     queried DNS name is not known e.g.: 'search google.com' results in any unknown
                                            #     'ping' (e.g. 'ping pigglywiggly') resolved by pinging google.
cat /etc/nsswitch.conf                      #change default order of resolution (hosts file first, then dns server)
nslookup www.google.com                     #query host name www.google.com from a dns server - DOES NOT CONSIDER /etc/hosts
dig www.google.com                          #like nslookup, but returns more details in a similar form as is stored at the dns server

ps aux            #list container processes - depends on user visibility
ip netns add <ns name>                      #create new network namespace on a host
ip netns                                    #list them

ip netns exec <ns name> ip link             #list interfaces within <namespace name>; prevents from viewing host interfaces
OR 
ip -n <ns name> link
SAME FOR
arp 
VS
ip netns exec <ns name> arp
OR
route 
VS
ip netns exec <ns name> route

#connect two namespaces together using virtual ethernet pair (aka virtual cable / pipe):
#Step 0: create each ns e.g.:
ip netns red
ip netns blue
#
#Step 1:
ip link add veth-<ns1 name (aka ns1)> type veth peer name veth-<ns2 name (aka ns2)>
#e.g.
ip link add veth-red type veth peer name veth-blue
#
#Step 2:
ip link set veth-<ns1> netns <ns1>
ip link set veth-<ns2> netns <ns2>
#e.g.
ip link set veth-red netns red
ip link set veth-blue netns blue
#
#Step 3:
ip -n <ns1> addr add <IP1> dev veth-<ns1>
ip -n <ns2> addr add <IP2> dev veth-<ns2>
#Step 4:
ip -n <ns1> link set <IP1> veth-<ns1> up
ip -n <ns2> link set <IP2> veth-<ns2> up
#Step 5: Test by pinging from ns1 to IP2:
ip netns exec <ns1> ping <IP2>
```
