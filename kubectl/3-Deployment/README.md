# Deployment

How to deal with workload on `Deploy` with `YAML`(Menifest-file)?

## Apply deployment to k8s for `DEV`

- Apply with Menifest-file

  ```bash
  # kubectl apply -f {menifest-file.yaml}
    kubectl apply -f codelab-3-deployment-dev.yaml
      # ingress.extensions/codelab-simple-ingress-dev created
      # service/codelab-simple-loadbalance-dev created
      # deployment.apps/codelab-simple-deployment-dev created
  ```

- Check

  ```bash
  # kubectl get {api-resources[,api-resources,..]}
    kubectl get ingress,service,deployment,replicaset,pod
      # NAME                                             HOSTS                 ADDRESS     PORTS   AGE
      # ingress.extensions/codelab-simple-ingress-dev    codelab.simple.dev    localhost   80      7m34s
      # ingress.extensions/codelab-simple-ingress-prod   codelab.simple.prod   localhost   80      65s

      # NAME                                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
      # service/codelab-simple-loadbalance-dev    ClusterIP   10.99.130.244   <none>        8878/TCP   7m34s
      # service/codelab-simple-loadbalance-prod   ClusterIP   10.105.51.77    <none>        80/TCP     65s
      # service/kubernetes                        ClusterIP   10.96.0.1       <none>        443/TCP    12d

      # NAME                                             READY   UP-TO-DATE   AVAILABLE   AGE
      # deployment.apps/codelab-simple-deployment-dev    2/2     2            2           7m34s
      # deployment.apps/codelab-simple-deployment-prod   0/4     4            0           65s

      # NAME                                                        DESIRED   CURRENT   READY   AGE
      # replicaset.apps/codelab-simple-deployment-dev-57b8665d84    2         2         2       7m34s
      # replicaset.apps/codelab-simple-deployment-prod-5b648787cb   4         4         0       65s

      # NAME                                                  READY   STATUS    RESTARTS   AGE
      # pod/codelab-simple-deployment-dev-57b8665d84-7h2bx    2/2     Running   0          7m33s
      # pod/codelab-simple-deployment-dev-57b8665d84-8sh5z    2/2     Running   0          7m34s
      # pod/codelab-simple-deployment-prod-5b648787cb-747vd   0/2     Pending   0          65s
      # pod/codelab-simple-deployment-prod-5b648787cb-bpwlw   0/2     Pending   0          65s
      # pod/codelab-simple-deployment-prod-5b648787cb-fdhkd   0/2     Pending   0          65s
      # pod/codelab-simple-deployment-prod-5b648787cb-t9jqm   0/2     Pending   0          65s
  ```

- Okay, Deployment for `DEV` is successful!

  ```bash
  # kubectl exec -ti {metadata.name} -c {containers.name} {command [-o opntion]}
    kubectl exec -ti codelab-simple-deployment-dev-{random-serials} -c web-nginx  /bin/bash
  
  # ex)
  # kubectl exec -ti codelab-simple-deployment-dev-57b8665d84-7v84p -c web-nginx  /bin/bash
    # root@codelab-simple-deployment-dev-57b8665d84-7v84p:/# echo 'Hello, nginx!'
    # Hello, nginx!
    # root@codelab-simple-deployment-dev-57b8665d84-7v84p:/#
  ```

- Test to `dev-ingress` at `codelab.simple.dev`(Host)

  ```bash
  # curl http[s]://{DOMAIN-HOST[:PORT]}[/URI] [-H {Host: routing.host}]
    curl http://localhost -H 'Host: codelab.simple.dev'
  ```

  You can see like this.

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

## Apply deployment to k8s for `PROD`

 ```bash
  # kubectl apply -f {menifest-file.yaml}
    kubectl apply -f codelab-3-deployment-prod.yaml
      # ingress.extensions/codelab-simple-ingress-prod created
      # service/codelab-simple-loadbalance-prod created
      # deployment.apps/codelab-simple-deployment-prod created
  ```

- Check

  ```bash
  # kubectl get {api-resources[,api-resources,..]}
    kubectl get ingress,service,deployment,replicaset,pod
      # NAME                                             HOSTS                 ADDRESS     PORTS   AGE
      # ingress.extensions/codelab-simple-ingress-dev    codelab.simple.dev    localhost   80      24m
      # ingress.extensions/codelab-simple-ingress-prod   codelab.simple.prod   localhost   80      4m46s

      # NAME                                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
      # service/codelab-simple-loadbalance-dev    ClusterIP   10.99.130.244   <none>        8878/TCP   24m
      # service/codelab-simple-loadbalance-prod   ClusterIP   10.107.34.119   <none>        80/TCP     4m46s
      # service/kubernetes                        ClusterIP   10.96.0.1       <none>        443/TCP    12d

      # NAME                                             READY   UP-TO-DATE   AVAILABLE   AGE
      # deployment.apps/codelab-simple-deployment-dev    2/2     2            2           24m
      # deployment.apps/codelab-simple-deployment-prod   3/3     3            3           4m46s

      # NAME                                                        DESIRED   CURRENT   READY   AGE
      # replicaset.apps/codelab-simple-deployment-dev-57b8665d84    2         2         2       24m
      # replicaset.apps/codelab-simple-deployment-prod-5b648787cb   3         3         3       4m46s

      # NAME                                                  READY   STATUS    RESTARTS   AGE
      # pod/codelab-simple-deployment-dev-57b8665d84-7h2bx    2/2     Running   0          24m
      # pod/codelab-simple-deployment-dev-57b8665d84-8sh5z    2/2     Running   0          24m
      # pod/codelab-simple-deployment-prod-5b648787cb-bc8hj   2/2     Running   0          4m46s
      # pod/codelab-simple-deployment-prod-5b648787cb-qcd6v   2/2     Running   0          4m46s
      # pod/codelab-simple-deployment-prod-5b648787cb-sdhgf   2/2     Running   0          4m46s

    kubectl get ingress,service,deployment,replicaset,pod --selector release:prod
      # NAME                                             READY   UP-TO-DATE   AVAILABLE   AGE
      # deployment.apps/codelab-simple-deployment-prod   3/3     3            3           6m27s

      # NAME                                                        DESIRED   CURRENT   READY   AGE
      # replicaset.apps/codelab-simple-deployment-prod-5b648787cb   3         3         3       6m27s

      # NAME                                                  READY   STATUS    RESTARTS   AGE
      # pod/codelab-simple-deployment-prod-5b648787cb-bc8hj   2/2     Running   0          6m27s
      # pod/codelab-simple-deployment-prod-5b648787cb-qcd6v   2/2     Running   0          6m27s
      # pod/codelab-simple-deployment-prod-5b648787cb-sdhgf   2/2     Running   0          6m27s
  ```

- Okay, Deployment for `PROD` is successful!

  ```bash
  # kubectl exec -ti {metadata.name} -c {containers.name} {command [-o opntion]}
    kubectl exec -ti codelab-simple-deployment-dev-{random-serials} -c web-nginx  /bin/bash
  # kubectl exec -ti codelab-simple-deployment-dev-5b648787cb-bc8hj -c web-nginx  /bin/bash
    # root@codelab-simple-deployment-dev-5b648787cb-bc8hj:/# echo 'Hello, nginx!'
    # Hello, nginx!
    # root@codelab-simple-deployment-dev-5b648787cb-bc8hj:/#
  ```

## Why use Deployment, does not ReplicaSet?

- The `Deployment` is more useful resource than the `ReplicaSet`.
  
  1. It can `make some ReplicaSet`.
  2. It can `record` deployment-affect.
  3. It can update with workload: `Rolling out` on availabilty.
  4. It can `roll-back` for successful-version of previous deployment `easy`.

## Record

  ```bash
  # kubectl apply -f {menifest-file.yaml} --record
    kubectl apply -f codelab-3-deployment-prod.yaml --record
      # ingress.extensions/codelab-simple-ingress-prod configured
      # service/codelab-simple-loadbalance-prod configured
      # deployment.apps/codelab-simple-deployment-prod configured

  # kubectl rollout history deployment {deployment-name}
    kubectl rollout history deployment codelab-simple-deployment-prod
      # deployment.apps/codelab-simple-deployment-prod 
      # REVISION  CHANGE-CAUSE
      # 1         kubectl apply --filename=3-Deployment/codelab-3-deployment-prod.yaml --record=true
  ```

- [Change replicaSet `3` to `4` in `codelab-3-deployment-prod.yaml`](./codelab-3-deployment-prod.yaml#L36). Then try below command.

  ```bash
  # kubectl apply -f {menifest-file.yaml} --record
    kubectl apply -f 3-Deployment/codelab-3-deployment-prod.yaml --record
      # ingress.extensions/codelab-simple-ingress-prod unchanged
      # service/codelab-simple-loadbalance-prod unchanged
      # deployment.apps/codelab-simple-deployment-prod configured

  # kubectl rollout history deployment {deployment-name}
    kubectl rollout history deployment codelab-simple-deployment-prod
      # deployment.apps/codelab-simple-deployment-prod 
      # REVISION  CHANGE-CAUSE
      # 1         kubectl apply --filename=3-Deployment/codelab-3-deployment-prod.yaml --record=true
  ```

- Why does `REVISION` is still `1` not `2`? Because `just changing number of pods is not recorded`. It doesn't mean to finish to this revision of replicaset. Instead, `REVISION` of record is affected that workload's container-image has been changed.

## Rolling Update with Record!

- Finally, here we are. In previous changes in ReplicaSet, it doesn't record. In this way, we will change container image.  
  [Change image version from `1.0.0` to `1.0.1`](./codelab-3-deployment-prod.yaml#L66).

  ```bash
  # kubectl apply -f {menifest-file.yaml} --record
    kubectl apply -f 3-Deployment/codelab-3-deployment-prod.yaml --record
      # ingress.extensions/codelab-simple-ingress-prod unchanged
      # service/codelab-simple-loadbalance-prod unchanged
      # deployment.apps/codelab-simple-deployment-prod configured
  
  # kubectl rollout history deployment {deployment-name}
    kubectl rollout history deployment codelab-simple-deployment-prod
      # deployment.apps/codelab-simple-deployment-prod 
      # REVISION  CHANGE-CAUSE
      # 1         kubectl apply --filename=3-Deployment/codelab-3-deployment-prod.yaml --record=true
      # 2         kubectl apply --filename=3-Deployment/codelab-3-deployment-prod.yaml --record=true
  ```

- How does affect to update? It works automatically rolliout.

  ```bash
  # kubectl get {api-resources[,api-resources,..]} --selector key=value
    kubectl get ingress,service,deployment,replicaset,pod --selector release=prod
      # NAME                                             READY   UP-TO-DATE   AVAILABLE   AGE
      # deployment.apps/codelab-simple-deployment-prod   3/4     3            3           37m

      # NAME                                                        DESIRED   CURRENT   READY   AGE
      # replicaset.apps/codelab-simple-deployment-prod-5b648787cb   2         2         2       37m
      # replicaset.apps/codelab-simple-deployment-prod-d4fc79585    3         3         1       32s

      # NAME                                                  READY   STATUS              RESTARTS   AGE
      # pod/codelab-simple-deployment-prod-5b648787cb-qcd6v   2/2     Running             0          37m
      # pod/codelab-simple-deployment-prod-5b648787cb-sdhgf   2/2     Running             0          37m
      # pod/codelab-simple-deployment-prod-d4fc79585-nh999    0/2     Pending             0          18s
      # pod/codelab-simple-deployment-prod-d4fc79585-t8lsz    0/2     ContainerCreating   0          32s
      # pod/codelab-simple-deployment-prod-d4fc79585-wcrtm    2/2     Running             0          32s

    kubectl get ingress,service,deployment,replicaset,pod --selector release=prod
      # NAME                                             READY   UP-TO-DATE   AVAILABLE   AGE
      # deployment.apps/codelab-simple-deployment-prod   4/4     4            4           39m

      # NAME                                                        DESIRED   CURRENT   READY   AGE
      # replicaset.apps/codelab-simple-deployment-prod-5b648787cb   0         0         0       39m
      # replicaset.apps/codelab-simple-deployment-prod-d4fc79585    4         4         4       2m20s

      # NAME                                                 READY   STATUS    RESTARTS   AGE
      # pod/codelab-simple-deployment-prod-d4fc79585-dxx72   2/2     Running   0          99s
      # pod/codelab-simple-deployment-prod-d4fc79585-nh999   2/2     Running   0          2m6s
      # pod/codelab-simple-deployment-prod-d4fc79585-t8lsz   2/2     Running   0          2m20s
      # pod/codelab-simple-deployment-prod-d4fc79585-wcrtm   2/2     Running   0          2m20s

    kubectl rollout status deployment codelab-simple-deployment-prod 
      # deployment "codelab-simple-deployment-prod" successfully rolled out
  ```

## Roll Back! Hurry..!

- REVISION of Deployment contain the detail contents

  ```bash
  # kubectl rollout history deployment {deployment-name} [--revision={number}]
    kubectl rollout history deployment codelab-simple-deployment-prod  --revision=1
      # deployment.apps/codelab-simple-deployment-prod with revision #1
      # Pod Template:
      #   Labels:       app=codelab
      #         pod-template-hash=5b648787cb
      #         release=prod
      #   Annotations:  kubernetes.io/change-cause: kubectl apply --filename=3-Deployment/codelab-3-deployment-prod.yaml --record=true
      #   Containers:
      #   web-nginx:
      #     Image:      dev2sponge/codelab-k8s-nginx:1.0.0
      #     Port:       80/TCP
      #     Host Port:  0/TCP
      #     Limits:
      #       cpu:      500m
      #       memory:   128M
      #     Requests:
      #       cpu:      250m
      #       memory:   64M
      #     Environment:
      #       BACKEND_HOST:     localhost:8080
      #     Mounts:     <none>
      #   was-tomcat:
      #     Image:      dev2sponge/codelab-k8s-tomcat:1.0.0
      #     Port:       8080/TCP
      #     Host Port:  0/TCP
      #     Limits:
      #       cpu:      500m
      #       memory:   128Mi
      #     Requests:
      #       cpu:      250m
      #       memory:   64Mi
      #     Environment:        <none>
      #     Mounts:     <none>
      #   Volumes:      <none>
  ```

- Do Rollback to previous revision!

  ```bash
  # kubectl rollout undo deployment {deployment-name}]
    kubectl rollout undo deployment codelab-simple-deployment-prod
      # deployment.apps/codelab-simple-deployment-prod rolled back

    kubectl get ingress,service,deployment,replicaset,pod --selector release=prod
      # NAME                                             READY   UP-TO-DATE   AVAILABLE   AGE
      # deployment.apps/codelab-simple-deployment-prod   3/4     2            3           50m

      # NAME                                                        DESIRED   CURRENT   READY   AGE
      # replicaset.apps/codelab-simple-deployment-prod-5b648787cb   2         2         0       50m
      # replicaset.apps/codelab-simple-deployment-prod-d4fc79585    3         3         3       13m

      # NAME                                                  READY   STATUS              RESTARTS   AGE
      # pod/codelab-simple-deployment-prod-5b648787cb-f8ccn   0/2     Pending             0          14s
      # pod/codelab-simple-deployment-prod-5b648787cb-qmr9t   0/2     ContainerCreating   0          14s
      # pod/codelab-simple-deployment-prod-d4fc79585-nh999    2/2     Running             0          13m
      # pod/codelab-simple-deployment-prod-d4fc79585-t8lsz    2/2     Running             0          13m
      # pod/codelab-simple-deployment-prod-d4fc79585-wcrtm    2/2     Running             0          13m
    
    kubectl get ingress,service,deployment,replicaset,pod --selector release=prod
      # NAME                                             READY   UP-TO-DATE   AVAILABLE   AGE
      # deployment.apps/codelab-simple-deployment-prod   3/4     4            3           51m

      # NAME                                                        DESIRED   CURRENT   READY   AGE
      # replicaset.apps/codelab-simple-deployment-prod-5b648787cb   4         4         3       51m
      # replicaset.apps/codelab-simple-deployment-prod-d4fc79585    0         0         0       14m

      # NAME                                                  READY   STATUS        RESTARTS   AGE
      # pod/codelab-simple-deployment-prod-5b648787cb-6rqpp   2/2     Running       0          44s
      # pod/codelab-simple-deployment-prod-5b648787cb-bkpm2   0/2     Pending       0          20s
      # pod/codelab-simple-deployment-prod-5b648787cb-f8ccn   2/2     Running       0          60s
      # pod/codelab-simple-deployment-prod-5b648787cb-qmr9t   2/2     Running       0          60s
      # pod/codelab-simple-deployment-prod-d4fc79585-wcrtm    0/2     Terminating   0          14m

    kubectl get ingress,service,deployment,replicaset,pod --selector release=prod
      # NAME                                             READY   UP-TO-DATE   AVAILABLE   AGE
      # deployment.apps/codelab-simple-deployment-prod   4/4     4            4           52m

      # NAME                                                        DESIRED   CURRENT   READY   AGE
      # replicaset.apps/codelab-simple-deployment-prod-5b648787cb   4         4         4       52m
      # replicaset.apps/codelab-simple-deployment-prod-d4fc79585    0         0         0       14m

      # NAME                                                  READY   STATUS    RESTARTS   AGE
      # pod/codelab-simple-deployment-prod-5b648787cb-6rqpp   2/2     Running   0          93s
      # pod/codelab-simple-deployment-prod-5b648787cb-bkpm2   2/2     Running   0          69s
      # pod/codelab-simple-deployment-prod-5b648787cb-f8ccn   2/2     Running   0          109s
      # pod/codelab-simple-deployment-prod-5b648787cb-qmr9t   2/2     Running   0          109s

    kubectl rollout status deployment codelab-simple-deployment-prod 
      # deployment "codelab-simple-deployment-prod" successfully rolled out

    # kubectl rollout history deployment {deployment-name}
    kubectl rollout history deployment codelab-simple-deployment-prod
      # deployment.apps/codelab-simple-deployment-prod 
      # REVISION  CHANGE-CAUSE
      # 1         kubectl apply --filename=3-Deployment/codelab-3-deployment-prod.yaml --record=true
      # 2         kubectl apply --filename=3-Deployment/codelab-3-deployment-prod.yaml --record=true
  ```

## Delete deployment

```bash
# kubectl delete -f {menifest-file.yaml}
  kubectl delete -f codelab-3-deployment-dev.yaml
  kubectl delete -f codelab-3-deployment-prod.yaml
```

### fin.

Congraturation! k8s-tutorial is done!

[Go back to `kubectl`]https://github.com/devJRL/CodeLab-Docker-Kubernetes/tree/master/kubectl#step2-hello-kubernetes)