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
ip link
ip addr add 192.168.1.10/24 dev eth0
route

```
