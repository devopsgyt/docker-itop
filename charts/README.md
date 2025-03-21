# HELM Chart

This is a helm chart used to easily deploy iTop to a K8S cluster using [HELM](https://helm.sh/).

## Volumes

In order for iTop to support being upgraded and being restarted, pvc are automatically created.

You can check the [template](./templates/pvc.yaml) for more info.

The folders that are saved are specified in the [deployment.yaml](./templates/deployment.yaml) file :

```yaml
volumeMounts:
  - mountPath: "/var/www/html/data"
    name: itop-data-pv
  - mountPath: "/var/www/html/env-production"
    name: itop-env-pv
  - mountPath: "/var/www/html/log"
    name: itop-log-pv
  - mountPath: "/var/www/html/conf"
    name: itop-conf-pv
```

## Values.yaml

You can edit the values.yaml file directly or use the set directive with the helm cli tool.

```yaml
image:
  repository: supervisions/itop
  pullPolicy: Always
  tag: 3.0
```

There also the environnement variables that are:

```yaml
environment:
  db_host: <mysql-server-url> "mysql-chart.database.svc.cluster.local"
  db_name: <mysql-db-name>
  db_pwd: <mysql-db-pwd>
  db_user: <mysql-db-user>
```

## Pod security

In order to allow the iTop service to access the files on the system, adding specific security context for the pod was needed, those can be found in the deployment.yaml file

```yaml
securityContext:
  runAsUser: 1000
  runAsGroup: 3000
  fsGroup: 2000
```

If you want to know more about the do and don't i invite you to read the k8s documentation [here](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

## Adding your own config-itop.php

You can create a configmap and add it to the pod deployment in order to have the config-itop.php file added before installing iTop.

This is not recommended.

## What is not added

A MySQL/MariaDB chart link in order to also have a db deployed to go with the iTop instance.
