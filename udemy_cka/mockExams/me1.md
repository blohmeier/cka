k get po --watch
#upgrade deployment
k set image deployment/<deployment-name> <container-name>=nginx:1.16
