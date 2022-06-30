```
#obtain pod label and accompanying service selector (should match; if not, one must change):
kubectl -n gamma describe pod mysql | grep -i label
kubectl -n gamma describe svc mysql-service | grep -i selector
```
