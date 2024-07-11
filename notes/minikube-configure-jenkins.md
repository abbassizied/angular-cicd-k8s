
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
apiVersion: v1
kind: Secret
metadata:
  name: jenkins-token
  annotations:
    kubernetes.io/service-account.name: jenkins
type: kubernetes.io/service-account-token

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
NAME            TYPE                                  DATA   AGE
jenkins-token   kubernetes.io/service-account-token   3      15s
$ kubectl describe secrets/jenkins-token
Name:         jenkins-token
Namespace:    default
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: jenkins
              kubernetes.io/service-account.uid: 5b466c7a-de60-4f26-a29c-a88b3878e5ef

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1111 bytes
namespace:  7 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IlQ4Y3ZpZEctVjVGSHpKZjcyc1YxVl9SbUhmUXpTLXlrVG4zWDFVUzA4RWMifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImplbmtpbnMtdG9rZW4iLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiamVua2lucyIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjViNDY2YzdhLWRlNjAtNGYyNi1hMjljLWE4OGIzODc4ZTVlZiIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmplbmtpbnMifQ.d4pcrXm0z93qLOhLwerzhj7AfUza5XhS4m-qPBlH3_FBv7OPKzqZ7j8WqFIhmAddLlBe0FNSc7OW2-wRlKkHcx12xlkgsP6z_5EAFsGjnY_vBRRHwA89Yp_JXYTu3GNuZRczjT7jp0jpzoqN2O86MxJR6O7FIeyABZuDy4QdTnENz1S6VsZYmYRX1nbcHzWgN1PN7QDtXZC4EeRE21Ee6lXfEsLzFiFcJF6U7cI5v0e2YyUMlzP0Ha-_8sP986BlezJdMeO-OHTlUNZ0trWy5bdAk3A4jjvS2maMZkPuIEz8TYyJg0D5hdMtqdiywapZnUQpR5mUyGlwNQkFfWK4kg
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
- Convert certificate info into base64 encoding and paste it in a field ( Kubernetes server certificate key)
```sh
$ cat C:/Users/Zied/.minikube/ca.crt | base64 -w 0; echo
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCakNDQWU2Z0F3SUJBZ0lCQVRBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwdGFXNXAKYTNWaVpVTkJNQjRYRFRJME1EY3dOVEV3TXpVeU1Gb1hEVE0wTURjd05ERXdNelV5TUZvd0ZURVRNQkVHQTFVRQpBeE1LYldsdWFXdDFZbVZEUVRDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTGJXCkI2SUhZMUNIUzR3OVNQaEZsTWxTamx6Rm52ZzhrRkphbWNDYUhsY0RlM2hDcmZXOVZtdVQ0bk9Hd2lhdmkwWFEKeVowaUFRaThmeUtjOE5VSGdaMXh0SFByU3BlNllSa2o0aVFYRzM0QkhaUTVlMGlJM1A5d3FtY1VaNU5Vd1FtTgpzdkJ4eEc5YjVOY2xlWGFEQUlXdTJpZU1aUUdSdzQrZnQzcGdhelN3QUJqQXlqN2FmbXExa0xFMVo3RUdGclU3CkpXL1JETm5VbDJrY0YvaUcvWDZia0NPTmtWRW16N0xBc2QxR0Rxb1NTL1BUaDNBMkwycWdTcDRUQ1ZQZkRpckgKczdQWkJ4cUU2WEVCdzRhRFdYcTlCVktwd1kxUmhSMFpSNzBGR2hSNTJKb2xJSUUyZmZaY1phOWFPQzJlTDBFUgpJdXVUQ2h6ZXViamJWUHFCSTdzQ0F3RUFBYU5oTUY4d0RnWURWUjBQQVFIL0JBUURBZ0trTUIwR0ExVWRKUVFXCk1CUUdDQ3NHQVFVRkJ3TUNCZ2dyQmdFRkJRY0RBVEFQQmdOVkhSTUJBZjhFQlRBREFRSC9NQjBHQTFVZERnUVcKQkJTYUNvU1hhK0ttLzZCOXB0MHdjLzQzN29kUWxEQU5CZ2txaGtpRzl3MEJBUXNGQUFPQ0FRRUFGZTNxWDFsVAozRXB1US9ZMy9pNXFGRG5DZnBpZWtocGNYTGlMa1NkcUZrbGxtMXM4UDRZSGhkNEx0OW1wRktGOUtKZ0EwSlQ1CnRtRTlkN2x3OU1wbWREbUJUVU9kT3ZCKy85NEU3QmtSd1NNZ05XV1pIWUdTdHMvNzlieDRSQitRSmxWMFNnRkMKb1hKSm9vV3lZM2lvdVlOdkRBY2Z4ZWxEdWgrR2Ftd2NCcjZ2aURDYXlLNzZxUER0ZGg2dU1iSXNPZjViUzVaTQpRRXpsdVdEWkFEUmtjUkdZL292NVh4a3hqNFFlTEYrRmJSYWJ1NXYwRHZ4YngwVUJ0Rlh4NHdtUkxXRGRnWVovCjI5a29QVnlGRlgzcStLdnZGNWdNTmJ3RUdEcG1xQWtSM0VMejdiMm5Uem5Bd1czN2F0b2NsUTcrdG9uQWZpQ0QKMkRKTnM1aEYrdEZFcnc9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
``` 
- 
- 
- 
```sh

``` 
- 
- 
```sh

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
```sh

``` 
- 