#### Update local cluster policy so that user admin can access all namespaces/projects
```
oc adm policy add-cluster-role-to-user cluster-admin system --as=system:admin
```
