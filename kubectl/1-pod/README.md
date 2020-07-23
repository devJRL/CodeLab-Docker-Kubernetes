# Pod

How to deal with container on `Pod` with `YAML`(Menifest-file)?

## Apply pod to k8s

- Apply with Menifest-file
  ```bash
  # kubectl apply -f {menifest-file.yaml}
    kubectl apply -f codelab-1-pod.yaml
      # pod/codelab-simple-pod created
  ```

- Check
  ```bash
  # kubectl get {api-resources[,api-resources,..]}
    kubectl get pods
      # NAME                     READY   STATUS    RESTARTS   AGE
      # pod/codelab-simple-pod   2/2     Running   0          4s
      #
      # NAME                  STATUS   ROLES    AGE   VERSION
      # node/docker-desktop   Ready    master   8d    v1.16.6-beta.0
  ```

## Log trace

```bash
# kubectl logs {metadata.name} -c {containers.name} [-f]
  kubectl logs codelab-simple-pod -c web-nginx -f  # tailing
  kubectl logs codelab-simple-pod -c web-tomcat    # casting
```

## Attach to container

- type1: on menifest file
  ```bash
  # [TYPE.1]
  # kubeclt exec -ti {menifest-file.yaml} {command} -c {containers.name}
    kubectl exec -ti  codelab-1-pod.yaml  /bin/bash -c web-nginx
      # root@codelab-simple-pod:/usr/local/nginx2# echo 'Hello, nginx!'
      # Hello, nginx!
      # root@codelab-simple-pod:/usr/local/nginx2# exit
  ```

- type2: on metadata
  ```bash
  # [TYPE.2]
  # kubectl exec -ti {metadata.name} -c {containers.name} {command [-o opntion]}
    kubectl exec -ti codelab-simple-pod -c was-tomcat  /bin/bash
      # root@codelab-simple-pod:/usr/local/tomcat# echo 'Hello, tomcat!'
      # Hello, tomcat!
      # root@codelab-simple-pod:/usr/local/tomcat# exit
  ```

## Delete pod

```bash
# kubectl delete -f {menifest-file.yaml}
  kubectl delete -f codelab-1-pod.yaml
```

### fin.

[Go back to `kubectl`]https://github.com/devJRL/CodeLab-Docker-Kubernetes/tree/master/kubectl#step2-hello-kubernetes)
