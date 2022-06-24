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
#Step 1: Create the 'cable':
ip link add veth-<ns1 name (aka ns1)> type veth peer name veth-<ns2 name (aka ns2)>
#e.g.
ip link add veth-red type veth peer name veth-blue
#
#Step 2: attach each 'interface' to the appropriate ns:
ip link set veth-<ns1> netns <ns1>
ip link set veth-<ns2> netns <ns2>
#e.g.
ip link set veth-red netns red
ip link set veth-blue netns blue
#
#Step 3: assign ip addresses to each 'interface' (i.e. each ns):
ip -n <ns1> addr add <IP1> dev veth-<ns1>
ip -n <ns2> addr add <IP2> dev veth-<ns2>
#e.g.
ip -n red addr add 192.168.15.1 dev veth-red
ip -n blue addr add 192.168.15.2 dev veth-blue
#
#Step 4: bring up each interface:
ip -n <ns1> link set <IP1> veth-<ns1> up
ip -n <ns2> link set <IP2> veth-<ns2> up
#e.g.
ip -n red link set 192.168.15.1 veth-red up
ip -n blue link set 192.168.15.2 veth-blue up
#Step 5: Test by trying a ping from ns1 to IP2:
ip netns exec <ns1> ping <IP2>
OR
ip netns exec <ns1> arp
#e.g.
ip netns exec red ping 192.168.15.2
OR
ip netns exec red arp

#create virtual switch within a host:
ip link add <virtual-switch-name> type bridge
ip link set dev <virtual-switch-name> up
#Step ?: similar to how attached 'cable' between two host interfaces before, attach 'cable' from each host interface to a v-switch interface. First must delete old cable. Then create cable for attaching each namespace to the virtual bridge:
ip -n red link del veth-red #'blue' end of cable deleted automatically
#Create cables for attaching each ns to an if on the virt bridge:
ip link add veth-red type veth peer name veth-red-br
ip link add veth-blue type veth peer name veth-blue-br
#Attach cables:
ip link set veth-red netns red
ip link set veth-red-br master v-net-0
ip link set veth-blue netns blue
ip link set veth-blue-br master v-net-0
#Set IP addresses for the 'interface' links and turn each of them on:
ip -n red addr add 192.168.15.1 dev veth-red
ip -n blue addr add 192.168.15.2 dev veth-blue
ip -n red link set veth-red up
ip -n blue link set veth-blue up
#Add ip addr to v-switch to allow host (e.g., eth0 at 192.168.1.2) to ping v-switch:
ip addr add 192.186.15.5/24 dev v-net-0
#Configure bridge to reach LAN network through eth0 port at the host:
ip netns exec blue ip route add 192.168.1.0/24 via 192.168.15.5


```
