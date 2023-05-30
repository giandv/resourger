# Project Idea

This project was born from the idea of wanting to synchronize resources on running pods in a totally automatic and transparent way :)

# Resourger Kubernetes Secret and ConfigMap synchronizer

Sometimes we want to use secrets in different namespaces, unfortunately, we can’t do without any helper operators or manual copying because in kubernetes secrets and configmaps are namespace. We can copy secrets and configmaps when we have a couple of namespaces and secrets. But when we have dozens of namespaces, it can be very complicated.

Resourger uses [kopf](https://github.com/nolar/kopf) python framework. Its easy to use.

## Deployment
It’s easy to use resourger on K8s. All we have to do is deploy configuration.yaml](https://raw.githubusercontent.com/giandv/resourger/main/configuration.yaml) to Kubernetes.

## Reload pod when ConfigMap or Secret upgraded
Add annotation `resourger/reload: "secret:example"` to pod, deployment, statefulset or replicaset template when we want sync secret example updated and busybox pod will be reload.

Note: For multiple secrets or configmaps:
`resourger/reload: "secret:example,secret:example2,configmap:example..."`

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: busybox
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      name: busybox
  template:
    metadata:
      labels:
        name: busybox
      annotations:
        resourger/reload: "secret:example"
    spec:
      containers:
        - name: busybox
          image: busybox
          command:
            - "sleep"
            - "1h"
```
## Triggers
 - When update config or secret
 - When create config or secret

## Exclude Namespaces

resourger Operator installs with cluster wide permissions, however you can optionally exclude namespaces you don't want to sync by setting the EXCLUDE_NAMESPACE environment variable.

`EXCLUDE_NAMESPACE` can be omitted entirely, or a comma separated list of k8s namespaces.

- `EXCLUDE_NAMESPACE=""` will watch for resources across the entire cluster.
- `EXCLUDE_NAMESPACE="foo"` will not sync resources in namespace foo.
- `EXCLUDE_NAMESPACE="foo,bar"` it will not sync resources in the foo and bar namespaces.

### Invite

Try this simple tutorial and take this project to the next level, you could create your own helm graphic and then have fun with replicasets.
