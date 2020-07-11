# Information of docker images

- `notice!` Just to use study. Don't be use in production.
- [dev2sponge/codelab-k8s-apache](https://hub.docker.com/repository/docker/dev2sponge/codelab-k8s-apache)
- [dev2sponge/codelab-k8s-tomcat](https://hub.docker.com/repository/docker/dev2sponge/codelab-k8s-tomcat)

## 1. How to build Apache (for WEB-Tier)

```bash
# MOVE TO PREPARE APACHE IAMGE
cd web-apache
```

### Build apache image
```bash
# BUILD
docker build -t codelab-k8s-apache:1.0.0 .
# CONFIRM
docker images | grep codelab
  # codelab-k8s-apache    1.0.0   6dbb65309318   12 seconds ago   166MB
```

### Check image with running container
```bash
# RUN ON SINGLE CONTAINER
docker run -dit -p 3333:80 \
--name codelab-web-apache \
codelab-k8s-apache:1.0.0
# CONFIRM
docker container ls | grep codelab
  # 4d5cd6954bf3   codelab-k8s-apache:1.0.0   "httpd-foreground"   1 seconds ago   Up 5 seconds   0.0.0.0:3333->80/tcp codelab-web-apache

  # CONNECT ON BROWSER, THEN YOU CAN SEE LIKE THIS
  # http://localhost:3333
    # WEB-Tier
    # Hello, docker!
    # Here is apache-container.
```

## 2. How to build Tomcat (for WAS-Tier)


```bash
# MOVE TO PREPARE TOMCAT IAMGE
cd was-tomcat
```

### Build apache image
```bash
# BUILD
docker build -t codelab-k8s-tomcat:1.0.0 .
# CONFIRM
docker images | grep codelab
  # codelab-k8s-tomcat    1.0.0   6dbb65309318   3 minutes ago   166MB
  # codelab-k8s-apache    1.0.0   6dbb65309318   9 seconds ago    166MB
```

### Check image with running container
```bash
# RUN ON SINGLE CONTAINER
docker run -dit -p 4444:8080 \
--name codelab-was-tomcat \
codelab-k8s-tomcat:1.0.0
# CONFIRM
docker container ls | grep codelab
  # 4d5cd6954bf3        codelab-k8s-tomcat:1.0.0   "httpd-foreground"   5 seconds ago       Up 4 seconds       80/tcp, 0.0.0.0:4444->8080/tcp   codelab-was-tomcat
  # 082ea076f72d        codelab-k8s-apache:1.0.0   "httpd-foreground"   26 minutes ago      Up 3 minutes       0.0.0.0:3333->80/tcp            codelab-web-apache

  # CONNECT ON BROWSER, THEN YOU CAN SEE LIKE THIS
  # http://localhost:4444/was.html
    # WAS-Tier
    # Hello, docker!
    # Here is tomcat-container.
```

Docker images are ready!

### 3. Remove containers

```bash
# STOP CONTAINER
docker stop codelab-web-apache
docker stop codelab-was-tomcat
# REMOVE CONTAINER
docker rm codelab-web-apache
docker rm codelab-was-tomcat
# CONFIRM
docker container ls -a | grep codelab
  # NOTHING.. DONE!
```

These images are already pushed in public docker repositories.

### 4. How to push docker images

```bash
# TAGGING IN LOCAL
docker tag codelab-k8s-apahce:1.0.0 dev2sponge/codelab-k8s-apahce:1.0.0
docker tag codelab-k8s-tomcat:1.0.0 dev2sponge/codelab-k8s-tomcat:1.0.0

# PUSH THIS LOCAL-TAG TO DOCKER-REPOSITORY
docker push dev2sponge/codelab-k8s-apache:1.0.0
docker push dev2sponge/codelab-k8s-tomcat:1.0.0
```

fin.
