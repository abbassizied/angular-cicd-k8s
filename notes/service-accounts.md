# Service Accounts

- A service account is a type of non-human account that, in Kubernetes, provides a distinct identity in a Kubernetes cluster. 
- Service accounts are different from user accounts, which are authenticated human users in the cluster. 
- **By default, user accounts don't exist in the Kubernetes API server; instead, the API server treats user identities as opaque data.**
- **When you create a cluster, Kubernetes automatically creates a ServiceAccount object named "default" for every namespace in your cluster.**
- If you delete the **default** ServiceAccount object in a namespace, the control plane replaces it with a new one. 
- **Use cases for Kubernetes service accounts**: As a general guideline, you can use service accounts to provide identities in the following scenarios:
    - An external service needs to communicate with the Kubernetes API server. For example, authenticating to the cluster as part of a CI/CD pipeline.
    - Your Pods need to communicate with an external service.
    - Authenticating to a private image registry using an imagePullSecret.
    - You use third-party security software in your cluster that relies on the ServiceAccount identity of different Pods to group those Pods into different contexts.
    - Your Pods need to communicate with the Kubernetes API server.
- **Every namespace has at least one ServiceAccount:** the default ServiceAccount resource, called default. You can list all ServiceAccount resources in your current namespace with:
```sh 
$ kubectl get serviceaccounts
```

