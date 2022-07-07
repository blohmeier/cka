```
k get po --watch

#upgrade deployment
k set image deployment/<deployment-name> <container-name>=<container-version> e.g.:
k set image deployment/nginx-deploy nginx=nginx:1.17
```
