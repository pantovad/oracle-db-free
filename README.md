# Oracle Database Free
[Oracle](http://www.oracle.com) Database Free—Developer Release is the same, powerful Oracle Database that businesses throughout the world rely on. It offers a full-featured experience and is packaged for ease of use and simple download—for free. See [Oracle Database Free](https://www.oracle.com/database/free)

## Getting started
A Helm chart is used for packaging the deployment yamls to simplify install in Kubernetes. Using [helm-charts/oracle-db](https://github.com/oracle/docker-images/tree/main/OracleDatabase/SingleInstance/helm-charts/oracle-db) as base and modified it for use with Oracle Database Free.
The OCI images supported are both official one from Oracle (https://container-registry.oracle.com/database/free) and the one provided by Gerald Venzl project (https://hub.docker.com/r/gvenzl/oracle-free). The Gerald Venzl's one is the defalt ne. The choice of the image is done with the image property in the chart vlues.
Please check the image docs for the supported configuration options.

Clone the repo and execute the following command to generate oracle-db-free-1.0.0.tgz:

```
$ helm package oracle-db-free
```

## Introduction

The Oracle Database Chart contains the Oracle Database Free. 

For more information on Oracle Database Free Release refer to https://www.oracle.com/database/free/

## Prerequisites

- Kubernetes 1.23+
- Helm 2.x or 3.x

## Installing the Chart

To install the chart with the release name `oracle-db-free`:

Helm 3.x syntax
```
$ helm install oracle-db-free oracle-db-free-1.0.0.tgz
```
Helm 2.x syntax
```
$ helm install --name oracle-db-free oracle-db-free-1.0.0.tgz
```

The command deploys Oracle Database on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `oracle-db-free` deployment:

Helm 3.x syntax
```
$ helm uninstall oracle-db-free
```
Helm 2.x syntax
```
$ helm delete oracle-db-free
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following tables lists the configurable parameters of the Oracle  Database chart and their default values.

| Parameter                            | Description                                | Default                                                    |
| -------------------------------      | -------------------------------            | ---------------------------------------------------------- |
| oracle_database (Gvenzl image)       | Application database (FREEPDB1 if not set) | unset                                                      |
| oracle_password                      | SYS, SYSTEM and PDB_ADMIN password         | Auto generated                                             |
| app_user (Gvenzl image)              | Application username                       | free                                                       |
| app_user_password (Gvenzl image)     | Application user password                  | Auto generated                                             |
| oracle_characterset (Oracl image)    | The character set used on DB creation      | AL32UTF8                                                   |
| nls_lang (venzl image)               | The client character set                   | AMERICAN_AMERICA.AL32UTF8                                  |
| persistence.size                     | Size of persistence storage                | 50g                                                        |
| persistence.storageClass             | Storage Class for PVC                      | "" (use the cluster default)                               |
| loadBalService                       | Create a load balancer service instead of NodePort | false                                              |
| image                                | Image to pull                              | gvenzl/oracle-free:latest                                  |
| imagePullPolicy                      | Image pull policy                          | IfNotPresent                                               |
| enable_archivelog (Oracl image)      | Set true to enable archive log mode when creating the database | false                                  |

ORACLE_SID is not configurable for Oracle Database 23c Free - Developer Release.
ORACLE_PDB can set to one or more comma separated values in Gvenzl image (use oracle_database value)ij order to create the pluggable databases. If not set the precreated FREEPDB1 delivered with the image is available 

|Environment variable   | Value     |
|---------------------- | --------- |
|ORACLE_SID             | FREE      |
|ORACLE_PDB             | FREEPDB1  |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

Helm 3.x syntax
```
$ helm install oracle-db-free --set oracle_pwd=<your_password> oracle-db-free-1.0.0.tgz
```
Helm 2.x syntax
```
$ helm install --name oracle-db-free --set oracle_pwd=<your_password> oracle-db-free-1.0.0.tgz
```

The above command sets the Oracle Database password to <your_password>.

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

Helm 3.x syntax
```
$ helm install oracle-db-free -f values.yaml oracle-db-free-1.0.0.tgz
```
Helm 2.x syntax
```
$ helm install --name oracle-db-free -f values.yaml oracle-db-free-1.0.0.tgz
```

> **Tip**: You can use the default [values.yaml](values.yaml)
 
 ## Image
 
 You can see more information about the image used on the [Oracle Container Registry](https://www.oracle.com/database/free/) page.

## Persistence

The [Oracle Database](https://www.oracle.com) image stores the Oracle Database data files  and configurations at the `/opt/oracle/oradata` path of the container.

Persistent Volume Claims are used to keep the data across deployments. 
See the [Configuration](#configuration) section to configure the PVC or to disable persistence.

## Connecting to the database

Connecting to the CDB as SYSDBA:

```
kubectl exec -it pods/pod-name -n oracle-db -- sqlplus sys/<your_password>@FREE as sysdba
```

Connecting to the PDB as SYSDBA:

```
kubectl exec -it pods/pod-name -n oracle-db -- sqlplus sys/<your_password>@FREEPDB1 as sysdba
```


> Your feedback is important to us! Connect with us if you have any questions or feedback on Oracle Database Free: https://community.oracle.com/hub
