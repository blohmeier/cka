```
k get po --watch

#upgrade deployment
k set image deployment/<deployment-name> <container-name>=<container-version> e.g.:
k set image deployment/nginx-deploy nginx=nginx:1.17
```

```
#Create new user and give access to cluster:
#search for 'rbac'; cut/paste CertSigningRequest
cat user.csr | base64 | tr -d "\n"
k create role <role-name> --verb=create,get,list,update,delete --resource=pods -n development
k auth can-i create pods --namespace=development --as <user-name>
k -n development create rolebinding john-developer --role=develper --user=john
```
