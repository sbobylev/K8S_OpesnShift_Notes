#### Update local cluster policy so that user admin can access all namespaces/projects
```
$ oc adm policy add-cluster-role-to-user cluster-admin system --as=system:admin
```
#### Install Helm in GKE
```
$ kubectl --namespace kube-system create serviceaccount tiller

$ kubectl create clusterrolebinding tiller-cluster-rule \
          --clusterrole=cluster-admin \
          --serviceaccount=kube-system:tiller

$ helm init --service-account tiller

$ helm repo update
```
#### Install nginx-ingress with RBAC enabled

```
helm install --name nginx-ingress stable/nginx-ingress --set rbac.create=true
```
