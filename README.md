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

#### Deploy a private images

```
$ kubectl create secret docker-registry gcr-credentials \
  --docker-server=https://gcr.io \
  --docker-username=_json_key \
  --docker-password='$(cat ~/gcp-service-account.json)' \
  --docker-email=user@example.com
  --namespace default
  
$ kubectl patch serviceaccount default --patch '{"imagePullSecrets": [{"name": gcr-credentials}]}' --namespace default
```

Optionally, deploy a pod referring to the secreted in the pod spec.

```
apiVersion: v1
kind: Pod
metadata:
  name: demo
spec:
  containers:
  - name: demo
    image: gcr.io/some-project/some-image:some-tag
  imagePullSecrets:
  - name: gcr-credentials

```
