# Step.2 Hello, Kubernetes!

This note will help you with questions about how to deal with the Cuvernetis resources.

## Pod

```bash
# APPLY POD BY MENIFEST FILE(.yaml)
kubectl apply -f codelab-1-pod.yaml

# CONFIRM
kubectl get pods
  # NAME                     READY   STATUS    RESTARTS   AGE
  # pod/codelab-simple-pod   2/2     Running   0          4s
  #
  # NAME                  STATUS   ROLES    AGE   VERSION
  # node/docker-desktop   Ready    master   8d    v1.16.6-beta.0

# REMOVE POD BY MENIFEST FILE(.yaml)
kubectl delte -f codelab-1-pod.yaml
```
