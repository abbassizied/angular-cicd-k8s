
# Minikube – configure Jenkins Kubernetes plugin
 
- Preparing a service account for kubernetes-plugin in minikube
```yml
# account.yaml
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
  namespace: default
---
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: jenkins
  namespace: default
rules:
- apiGroups: [""]
  resources: ["pods","services"]
  verbs: ["create","delete","get","list","patch","update","watch"]
- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["create","delete","get","list","patch","update","watch"]
- apiGroups: [""]
  resources: ["pods/exec"]
  verbs: ["create","delete","get","list","patch","update","watch"]
- apiGroups: [""]
  resources: ["pods/log"]
  verbs: ["get","list","watch"]
- apiGroups: [""]
  resources: ["secrets"]
  verbs: ["get"]
- apiGroups: [""]
  resources: ["persistentvolumeclaims"]
  verbs: ["create","delete","get","list","patch","update","watch"]
 
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: jenkins
  namespace: default
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: jenkins
subjects:
- kind: ServiceAccount
  name: jenkins
---
# Allows jenkins to create persistent volumes
# This cluster role binding allows anyone in the "manager" group to read secrets in any namespace.
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: jenkins-crb
subjects:
- kind: ServiceAccount
  namespace: default
  name: jenkins
roleRef:
  kind: ClusterRole
  name: jenkinsclusterrole
  apiGroup: rbac.authorization.k8s.io
---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  # "namespace" omitted since ClusterRoles are not namespaced
  name: jenkinsclusterrole
rules:
- apiGroups: [""]
  resources: ["persistentvolumes"]
  verbs: ["create","delete","get","list","patch","update","watch"]
``` 
- Apply above file, it will create service account
```sh
$ kubectl apply -f k8s/account.yaml
serviceaccount/jenkins created
role.rbac.authorization.k8s.io/jenkins created
rolebinding.rbac.authorization.k8s.io/jenkins created
clusterrolebinding.rbac.authorization.k8s.io/jenkins-crb created
clusterrole.rbac.authorization.k8s.io/jenkinsclusterrole created
``` 
- Get token:
```sh
$ kubectl get secrets
$ 
$ 
$ 
``` 
- 
- In ```C:\Users\Zied\.kube``` folder there is file named **config**. We’ll need **server** and **client-certificate value**
```yml
apiVersion: v1
clusters:
- cluster:

... 

- cluster:
    certificate-authority: C:\Users\Zied\.minikube\ca.crt
    extensions:
... 

- name: minikube
  user:
    client-certificate: C:\Users\Zied\.minikube\profiles\minikube\client.crt
    client-key: C:\Users\Zied\.minikube\profiles\minikube\client.key
``` 
- 
- 
- 
- 
- 
- 
- 
- 
    - 
    - 
    - 
    - 
- 
- 
- 
- 
- 
```sh

``` 
- 