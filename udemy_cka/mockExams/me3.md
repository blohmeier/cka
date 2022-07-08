create sa 'pvviewer', grant it access to list all pvs in the cluster by creating ClusterRole 'pvviewer-role' and ClusterRoleBinding 'pvviewer-role-binding'.
```
k create sa pvviewer
#k create clusterrole pvviewer-role --verb=list --resource=per #"park it" to confirm pv wording
k api-resources | grep persistent
k create clusterrole pvviewer-role --verb=list --resource=persistentvolumes

```
