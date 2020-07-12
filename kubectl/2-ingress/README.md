# Ingress

How to expose container on `Pod` in k8s?

## Prepare Nginx-Ingress-Controller

- Apply `Ingress Cluster` on Menifest-file from Kubernetes Group.
  ```bash
  # kubectl apply -f {menifest-file.yaml}
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/cloud/deploy.yaml
  ```

- Check Nginx-Igress-Controller is ready. ([more info](https://kubernetes.github.io/ingress-nginx/deploy/#installation-guide))
  ```bash
    kubectl wait --timeout=120s \
      --for=condition=ready pod \
      --namespace ingress-nginx \
      --selector=app.kubernetes.io/component=controller
  ```

- If you done, your cluster like below.
  ```bash
  # kubectl get [namepase|ns]
    kubectl get namespace | grep ingress
    # ingress-nginx     Active   3m
  ```

## Apply Ingress & Service

- Apply `Igress` with Menifest-file
  ```bash
  # kubectl apply -f {menifest-file.yaml}
    kubectl apply -f codelab-2-ingress.yaml
      # service/codelab-simple-loadbalance created
      # ingress.extensions/codelab-simple-ingress created
  ```

- Apply `Pod` in previouce step with Menifest-file
  ```bash
  # kubectl apply -f {menifest-file.yaml}
    kubectl apply -f ../1-pod/codelab-1-pod.yaml
      # pod/codelab-simple-pod created
  ```

- Check
  ```bash
  # kubectl get {api-resources[,api-resources,..]}
    kubectl get pods,service,namespace | egrep "NAME|codelab|ingress"
      # NAME                     READY   STATUS    RESTARTS   AGE
      # pod/codelab-simple-pod   2/2     Running   0          2m1s
      # NAME                                 TYPE        CLUSTER-IP       # EXTERNAL-IP   PORT(S)   AGE
      # service/codelab-simple-loadbalance   ClusterIP   10.107.126.155   <none>        80/TCP    6m34s
      # NAME                        STATUS   AGE
      # namespace/ingress-nginx     Active   12m

    kubectl get ingress
      # NAME                     HOSTS            ADDRESS     PORTS   AGE
      # codelab-simple-ingress   codelab.simple   localhost   80      84s
  ```

## Reqeust on HTTP

- If you do this in browser, your ingress cannot serve application page. Becasuse ingress-nginx cannot reseive `certain host` with `GET` request when `curl` command.

- Request to application on `curl` command.
  ```bash
    curl http://localhost -H 'Host: codelab.simple'
  ```

- Reponse from apllication for `curl` command.
  ```html
  <!DOCTYPE html>
  <html>
  <head>
    <title>CodeLab-Docker-Kubernetes</title>
  </head>
  <body>
    <h1>WEB-Tier</h1>
    <h2>Hello, docker!</h2>
    <h3>Here is apache-container.</h3>
  </body>
  </html>
  ```

- The key of ingress is `Host`. To understand, check `-H` option of `curl` command with `host` rule of [`codelab-2-ingress.yaml`](https://github.com/devJRL/CodeLab-Docker-Kubernetes/blob/master/kubectl/2-ingress/codelab-2-ingress.yaml#L20).

## Delete

```bash
# kubectl delete -f {menifest-file.yaml}
  kubectl delete -f codelab-2-ingress.yaml
    # service "codelab-simple-loadbalance" deleted
    # ingress.extensions "codelab-simple-ingress" deleted
  kubectl delete -f codelab-1-pod.yaml
    # pod "codelab-simple-pod" deleted
```

### fin.

[Go back to `kubectl`]https://github.com/devJRL/CodeLab-Docker-Kubernetes/tree/master/kubectl#step2-hello-kubernetes)
